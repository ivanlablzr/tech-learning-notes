---
type: note
tags: [cybersecurity, hardening, secure-coding]
---


Reducing a system's **attack surface** and enforcing secure defaults — across the application, the OS, user accounts, and servers. The unifying idea: remove what you don't need, run with least privilege, and make exploitation harder even when bugs exist.

## 1. Application hardening

**Reduce attack surface:** disable debug interfaces/admin panels/legacy protocols in production, remove dead dependencies (they still get CVEs), run as a dedicated low-privilege user (never root/SYSTEM), bind to `127.0.0.1` when external access isn't needed, and gate management APIs behind separate auth.

**Memory-protection mechanisms** (defense against memory-corruption exploits):

| Mechanism | What it does | Bypassed by → mitigated by |
|---|---|---|
| **ASLR** | randomizes stack/heap/lib addresses (`randomize_va_space=2`; needs PIE) | info leaks |
| **DEP/NX** | marks data memory non-executable | ROP → CFI |
| **Stack canaries** | detects stack-overflow before return (`-fstack-protector-strong`) | — |
| **CFI** | restricts indirect calls/returns to valid targets (Clang CFI, Windows CFG) | defeats ROP/JOP |
| **Intel CET / shadow stack** | hardware-validated return addresses | — |
| **RELRO** | makes the GOT read-only (`-z relro -z now` = full) | GOT overwrite |

**Secure compilation** (GCC/Clang): `-fstack-protector-all -pie -fPIE -D_FORTIFY_SOURCE=2 -Wl,-z,relro -Wl,-z,now -Wl,-z,noexecstack`. Verify with `checksec` / `hardening-check`. Harden the servers/DBs themselves too (remove default content, enforce TLS, disable directory listing, restrict network access, encrypt at rest, audit access).

## 2. Secure coding

The discipline behind the OWASP mitigations ([[Internet & Application Security#OWASP Top 10|OWASP Top 10]]):

- **Input validation** — allowlist, validate type/length/format server-side; never trust the client.
- **Parameterized queries / prepared statements** — the definitive fix for injection (never string-concatenate SQL or pass user input to a shell).
- **Output encoding** — context-aware (HTML/JS/URL) to stop XSS.
- **Secure error handling** — log details server-side, return generic messages (no stack traces, DB types, or paths).
- **Secrets** — never hard-code; use a vault/secrets manager and environment injection.
- **Dependency hygiene** — pin versions with hash verification; track an **SBOM**.

**Security testing:** **SAST** (scans source for patterns — early, some false positives), **DAST** (attacks the running app — finds runtime/config bugs), **IAST** (instrumented hybrid), **SCA** (flags known-vulnerable dependencies — Snyk, Dependency-Check, Dependabot), and **secret scanning** (gitleaks, trufflehog). Wire these into CI/CD so every build is checked.

## 3. OS hardening

Benchmark against the **CIS Benchmarks** (free consensus configs; Level 1 = broadly safe, Level 2 = high-security) — automated scanners: OpenSCAP, Lynis, CIS-CAT.

**Attack-surface reduction:** remove unused packages (`apt purge telnetd vsftpd rsh-server`), disable/mask services (`systemctl mask`), and kill legacy cleartext protocols:

| Protocol | Risk | Replace with |
|---|---|---|
| Telnet, FTP, rsh/rlogin | cleartext creds / no auth | SSH, SFTP |
| NIS | weak broadcast auth | LDAP/Kerberos |
| SNMPv1/v2c | plaintext community string | SNMPv3 authPriv |

**Kernel hardening** (`/etc/sysctl.d/`): SYN cookies (`tcp_syncookies=1`), reverse-path filtering (`rp_filter=1`), drop source routing & ICMP redirects, full ASLR (`randomize_va_space=2`), restrict kernel pointers/dmesg/ptrace (`kptr_restrict=2`, `yama.ptrace_scope=1`), disable `suid_dumpable`.

**Filesystem:** restrictive mount options in `/etc/fstab` — `noexec,nosuid,nodev` on `/tmp`, `/var/tmp`, `/dev/shm`; sticky bit (`+t`) on world-writable dirs; encrypt data at rest ([[Data Encryption#5. Data at rest|FDE]]).

**Mandatory Access Control** beyond Unix permissions: **SELinux** (label-based, RHEL) or **AppArmor** (path-based, Ubuntu) confine processes so even a compromised service can't exceed its policy. On Windows: GPO/Security Baselines, Defender Application Control (WDAC), AppLocker, Attack Surface Reduction rules.

## 4. User & account hardening

- **Least privilege** — minimal rights per account; service accounts with `nologin` shells; no shared admin accounts.
- **Sudo, not root** — granular `sudoers`, full command logging; disable direct root login (`PermitRootLogin no`).
- **Strong auth** — enforce MFA, key-based SSH (disable password auth), Fail2Ban against brute force; password policy per [[Information Security & Access#2. Passwords & strong authentication|NIST 800-63B]].
- **Account lifecycle** — disable/remove dormant and default accounts, expire credentials, review group membership; **PAM** for privileged accounts (JIT, vaulting, session recording).
- **Audit** — enable `auditd`/Windows audit policy; ship logs off-host.

## 5. Server & patch hardening

- **Patch management** — the single highest-impact control. Inventory assets, subscribe to advisories (NVD, CISA KEV), test then deploy on an SLA (critical CVEs fast), and automate (WSUS/SCCM, `unattended-upgrades`, Ansible). Unpatched services cause most breaches (Equifax/Struts, EternalBlue).
- **Minimal install** — only required roles/packages; close unused ports (host firewall default-deny: `ufw`/`firewalld`/iptables).
- **Secure remote management** — SSH hardened (key-only, restricted ciphers, non-default config), management on a separate network, bastion/jump hosts.
- **Hardened baselines & immutability** — golden images, IaC with security checks (Terraform + Trivy), container hardening (minimal base images, non-root, read-only FS, no capabilities), and config drift detection.
- **Monitoring & integrity** — file integrity monitoring (AIDE, Tripwire), centralized logging to SIEM, time sync (authenticated NTP).

Related: [[06 Cybersecurity|domain overview]] · [[Internet & Application Security#OWASP Top 10|secure coding / OWASP]] · [[Endpoint Security]] · [[Network Security#6. Monitoring & device hardening|device hardening]] · [[Information Security & Access#1. Identity & Access Management (IAM)|IAM]] · [[04 Operating Systems]]
