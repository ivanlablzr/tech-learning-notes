---
type: note
tags: [os, windows, sysadmin]
---



A working model of Windows for administration, troubleshooting, and security. PowerShell is the primary tool; the registry and event log are the two databases you must understand.

## 1. Architecture & boot

```
User apps → Win32 API → subsystem DLLs (kernel32/advapi32) → ntdll.dll (Native API) →
ntoskrnl.exe (Executive: Process/Memory/IO managers, Security Reference Monitor, Object Manager, Registry) → HAL → Hardware
```
**Kernel mode (Ring 0)** runs drivers — a crash = BSOD. **User mode (Ring 3)** reaches the kernel via syscalls through `ntdll`. Per-process structures: `_EPROCESS` (kernel: token, handles), `PEB`/`TEB` (user: modules, env, TLS). All `.exe/.dll/.sys` use the **PE** format.

**Boot:** UEFI + Secure Boot → `bootmgr` reads **BCD** → `winload.efi` loads `ntoskrnl` + drivers + SYSTEM hive → `smss.exe` → `csrss`/`wininit` (→ `services.exe`, `lsass.exe`) / `winlogon` → login → token created → `explorer.exe`.

> **Forensic baseline:** `System`=PID 4; single instances of `smss/wininit/winlogon/services/lsass` from `C:\Windows\System32\` only; many `svchost` (each under `services.exe`). Any of these from a wrong path, with an unexpected parent, or duplicated = investigate (malware spoofs these names). `lsass.exe` is the top credential-theft target.

## 2. Key directories

`Program Files` / `Program Files (x86)` (64/32-bit apps) · `ProgramData` (machine app data) · `Users\<u>\AppData\{Local,LocalLow,Roaming}` (Roaming holds creds/persistence) · `Windows\System32` (64-bit core), `SysWOW64` (32-bit), `System32\config` (registry hives), `System32\winevt\Logs` (.evtx), `Prefetch` (execution history), `Temp` (malware staging). Autorun folders: `…\Start Menu\Programs\Startup\` (per-user and all-users). Local DNS override: `System32\drivers\etc\hosts`.

## 3. Filesystems

**NTFS** (use for OS + data): every file is an **MFT** record; features = **ACL permissions (DACL)**, journaling (`$LogFile`), **VSS** snapshots, **EFS** per-file encryption, **ADS** (hidden streams `file:stream`; `Zone.Identifier` marks downloads — abused by malware), symlinks/junctions, quotas. Forensic artifacts: `$MFT`, `$UsnJrnl` (change journal). Inspect: `Get-Item f -Stream *`, `dir /r`, `streams.exe`. Repair: `chkdsk /f /r`, `sfc /scannow`, `DISM /Online /Cleanup-Image /RestoreHealth`.

| | FAT32 | exFAT | NTFS | ReFS |
|---|---|---|---|---|
| Max file | 4 GB | 128 PB | 16 EiB | 35 PB |
| Permissions / journaling | ✗/✗ | ✗/ltd | ✓/✓ | ✓/✓ |
| Boot volume | ✓ | ✗ | ✓ | ✗ |
| Best use | USB/SD | external | OS+data | server (Hyper-V, S2D) |

## 4. Processes & services

```powershell
Get-Process | Sort CPU -Desc | Select Name,Id,CPU
Get-CimInstance Win32_Process | Select Name,ProcessId,ParentProcessId,CommandLine   # parent + cmdline
Get-FileHash proc.exe -Algorithm SHA256        # hash unknowns → VirusTotal
Stop-Process -Id 1234 -Force
```
**Services** are managed by the **SCM** (`services.exe`), mostly hosted in `svchost.exe`. `Get-Service`, `Start/Stop/Restart-Service`, `Set-Service -StartupType Disabled`; `tasklist /svc` maps services to svchost PIDs; `Get-WinEvent System | Id 7045` = new service installed (persistence indicator). Disable **Spooler** (PrintNightmare), **RemoteRegistry**, **Telnet/FTP** unless needed; never disable `EventLog`, `lsass`.

## 5. Registry

Hierarchical config database. Hives: **HKLM** (system: `SAM`,`SECURITY`,`SYSTEM`,`SOFTWARE`), **HKCU** (user `NTUSER.DAT`), HKU, HKCR (associations), HKCC. Value types `REG_SZ/DWORD/QWORD/BINARY/MULTI_SZ`.

**Autorun / persistence keys:** `HKLM\…\CurrentVersion\Run` (boot, all users), `HKCU\…\Run` (login), `…\Winlogon` (Shell/Userinit hijack), `HKLM\SYSTEM\CurrentControlSet\Services`. **Security-sensitive:** `HKLM\SAM` (hashes — needs SYSTEM), `HKLM\SECURITY\Policy\Secrets` (LSA secrets), `WDigest` `=1` (plaintext creds in memory).
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
```

## 6. Users, permissions & privileges

Every principal has a **SID** (permissions reference SIDs, not names). Well-known: `S-1-5-18` SYSTEM, `…-500` local Administrator, `S-1-1-0` Everyone, `S-1-5-11` Authenticated Users. Account types: local (SAM), domain (AD), service (LocalSystem/Service, gMSA with auto-rotating password).

**NTFS ACLs:** a security descriptor holds the **DACL** (Allow/Deny ACEs per SID) and **SACL** (audit). Rules: explicit **Deny wins**; explicit beats inherited; effective access = union of Allows minus Denies. Tools: `Get-Acl`, `icacls path /grant user:F`, `takeown`.

**UAC & integrity (MIC):** processes run at Low/Medium/High/System integrity; admins run Medium until elevated. A Low-integrity (sandboxed) process can't write to System32 even with code execution. The **access token** (built by lsass at login) carries SIDs + privileges; dangerous ones:

| Privilege | Abuse |
|---|---|
| `SeImpersonatePrivilege` | Potato exploits → SYSTEM |
| `SeDebugPrivilege` | Dump lsass (credentials) |
| `SeBackup/RestorePrivilege` | Read/write any file bypassing ACLs (SAM hive) |

## 7. CLI & PowerShell essentials

`systeminfo`, `whoami /all`/`/priv`, `ipconfig /all`, `netstat -ano`, `tasklist /svc` (CMD). PowerShell: `Get-ChildItem -Recurse`, `Select-String` (grep), `Get-NetTCPConnection -State Listen`, `Test-NetConnection host -Port 443`, `Get-LocalGroupMember Administrators`. **Execution policy:** `Set-ExecutionPolicy RemoteSigned`; one-off `-ExecutionPolicy Bypass`. **Remoting (WinRM):** `Enter-PSSession`/`Invoke-Command -ComputerName`. **WMI/CIM** exposes the whole OS (`Get-CimInstance Win32_*`); watch for **WMI event-subscription persistence** (`root\subscription` __EventFilter/__Consumer/__Binding) — a fileless malware technique.

## 8. Event logs (the audit trail)

Logs: **Security** (auth, privilege, policy), **System** (drivers/services), **Application**, and structured **Applications and Services Logs** (PowerShell, Sysmon, Defender). Read with `Get-WinEvent -FilterHashTable @{LogName='Security';Id=4625}` or `wevtutil`.

**Key event IDs:** 4624 logon (Type 2=interactive, 3=network, 10=RDP), 4625 failed logon, 4672 admin logon, 4688 process created, 4720 user created, 4728/4732 added to group, 4740 lockout, **7045** new service, **4698** new scheduled task, **1102** security log cleared (almost always malicious). Persistence hunting = watch 7045 + 4698; never-cleared 1102.

## 9. Scheduled tasks, firewall, Group Policy, AD

- **Task Scheduler** (`taskschd.msc`): trigger + action; heavily abused for persistence. Audit for tasks running outside `C:\Windows\` and at logon/boot; Event 4698 = new task.
- **Windows Firewall** (`wf.msc`): stateful, per-profile (Domain/Private/Public), Block overrides Allow. `Get/Set/New-NetFirewallRule`, `netsh advfirewall`.
- **Group Policy** enforces config at scale, applied **LSDOU** (Local→Site→Domain→OU, last wins). `gpupdate /force`, `gpresult /h report.html`, `rsop.msc`.
- **Active Directory** = centralized identity (Kerberos TGT/TGS). DC stores **NTDS.dit** (all objects + hashes); **SYSVOL** holds GPOs; **FSMO** roles; AD depends on DNS SRV records. Manage via RSAT (`Get-ADUser`, `Get-ADGroupMember "Domain Admins" -Recursive`).

## 10. Troubleshooting & hardening

**Diagnose:** Reliability Monitor / Event Viewer first. Network: `ipconfig /all` → `ping 127.0.0.1` → gateway → `8.8.8.8` → `nslookup` → port; reset stack with `netsh int ip reset && netsh winsock reset && ipconfig /flushdns`. Boot: Recovery Env `bootrec /fixmbr|/fixboot|/rebuildbcd`, Safe Mode via `bcdedit`. Corruption: `sfc /scannow` then `DISM …/RestoreHealth`. BSOD: read `C:\Windows\Minidump` (e.g. `DRIVER_IRQL_NOT_LESS_OR_EQUAL` = bad driver, `CRITICAL_PROCESS_DIED` = lsass/winlogon → malware scan).

**Hardening checklist (priority):** disable **SMBv1** + enable SMB signing; disable **LLMNR/NetBIOS** (anti-relay); Defender + Controlled Folder Access; **BitLocker** (TPM+PIN); **LAPS** (kills shared local-admin password reuse); password 14+ & lockout 5/15; **NTLMv2 only**; disable Spooler/RemoteRegistry/Telnet; advanced audit policy; **PowerShell Script Block Logging + Transcription**; **Sysmon** (SwiftOnSecurity config); **WDAC/AppLocker** (WDAC is kernel-enforced, beats local admin); **NLA for RDP**; benchmark with CIS-CAT.

**Essential tools:** Sysinternals Suite (Process Explorer, Autoruns, ProcMon, TCPView, Streams), plus `resmon`, `perfmon`, `eventvwr`, `secpol.msc`, `taskschd.msc`, `icacls`, `bcdedit`.

Related: [[Linux]] · [[04 Operating Systems|OS concepts]] · [[System Hardening]] · [[06 Cybersecurity|Ethical Hacking]] · [[Network Security]]
