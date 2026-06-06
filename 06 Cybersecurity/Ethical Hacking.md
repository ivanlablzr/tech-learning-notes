---
type: note
tags: [cybersecurity, redteam, pentesting, offensive]
---


The offensive reference — authorized attack simulation to find weaknesses before real adversaries do. Covers the methodology, the technique catalog by surface, the credential-attack playbook, and a realistic path into the field. (Defensive counterparts are in [[Security Operations & IR]] and [[Network Security]].)

## 1. Engagement basics

Everything starts with written authorization. The **Rules of Engagement (RoE)** define the legal boundary: scope (IP ranges, domains, cloud accounts, physical sites — explicitly in/out), time windows and blackouts, emergency contacts ("get-out-of-jail" call tree), and data handling. Confirm authorization covers the **full chain** (named phishing targets, cloud-provider testing policy, third-party/CDN exclusions).

| Engagement | Start position | Goal |
|---|---|---|
| **External pentest** | no creds, outside | initial access → escalate |
| **Internal / assumed breach** | low-priv domain user | Domain Admin, data exfil |
| **Red team** (APT sim) | zero knowledge | objective-based, evade detection |
| **Purple team** | coordinated with blue | validate TTPs ↔ detections |

Pick a **threat-actor persona** (nation-state = slow/stealthy; cybercriminal = fast/noisy) — it sets your OPSEC posture. Red-team **OPSEC**: dedicated VPS infra, C2 behind redirectors (never expose the C2 IP), aged categorized domains, out-of-band team comms, persona separation. Frameworks: **PTES**, OWASP Testing Guide, NIST SP 800-115, CREST.

## 2. The methodology

A repeatable kill-chain from outside-in to objective:

1. **Reconnaissance** — *passive OSINT* first (no target interaction): subdomain enum from certificate transparency/passive sources (Amass, subfinder, crt.sh), `theHarvester`, Shodan for exposed assets, and **GitHub dorking** for leaked secrets (`trufflehog`, `.env`/`id_rsa`/API keys). Then *active*: DNS, employee/email harvesting for social engineering.
2. **Scanning & enumeration** — map live hosts, ports, services, and versions (`nmap`, masscan), then probe each service deeply (web dirs with `ffuf`/gobuster, SMB/LDAP/SNMP enumeration, vuln scan with Nessus/nuclei).
3. **Exploitation / initial access** — turn a finding into a foothold: web exploits (SQLi, file upload, SSRF, deserialization), service CVEs (Metasploit, public PoCs), default/weak creds, or **phishing** (GoPhish + macro/HTML-smuggling/ISO-LNK).
4. **Post-exploitation** — establish a stable shell, **persistence** (scheduled task, run key, service), and **privilege escalation** (Linux: `sudo -l`/GTFObins, SUID, kernel exploits like PwnKit; Windows: token abuse, unquoted service paths, **LSASS dump** via Mimikatz/procdump).
5. **Lateral movement** — harvest creds and pivot (Pass-the-Hash, PsExec/WMI/WinRM, BloodHound to map the path to Domain Admin).
6. **Actions on objective & reporting** — collect/exfil per scope, then deliver the real product: a **clear report** with findings, business risk, reproduction steps, and prioritized remediation.

## 3. Technique catalog by surface

- **Web** — the OWASP Top 10 in practice: injection (`sqlmap`), XSS, SSRF→cloud-metadata, IDOR/BOLA, auth/JWT flaws, file-upload webshells (toolkit: Burp Suite). Detail in [[Internet & Application Security#OWASP Top 10|OWASP Top 10]].
- **OS / privilege escalation** — enumerate with LinPEAS/WinPEAS; abuse misconfigs (sudo, SUID, services, scheduled tasks, registry).
- **Active Directory** — the enterprise crown jewel: BloodHound for attack paths, **Kerberoasting**/AS-REP roasting, Pass-the-Hash/Ticket, **DCSync**, Golden/Silver tickets, ADCS (ESC1) abuse (see §4).
- **Mobile** — static (APK/IPA decompilation, hardcoded secrets) + dynamic (Frida hooking, traffic interception, insecure storage); OWASP MASVS.
- **Hardware / electronics** — UART/JTAG debug access, SPI-flash firmware extraction, glitching/fault injection, bus sniffing (Bus Pirate, logic analyzer).
- **Radio / RF** — SDR (HackRF, RTL-SDR) for replay/jamming/analysis of key fobs, RFID/NFC (Proxmark), and wireless (Wi-Fi handshake capture/cracking, BLE).
- **Physical access control** — badge cloning, lock bypass, tailgating, and defeating door/RFID systems (these tie to social engineering and on-site red-team ops).

## 4. Credential-attack playbook (Windows/AD)

The path from a foothold to total domain control — and how each step is caught:

| Technique | What it steals/forges | Detection | Counter |
|---|---|---|---|
| **Password spray** | valid creds (1 pw × many users, avoids lockout) | distributed low-rate failures | smart lockout, MFA |
| **LSASS dump** | NTLM hashes of logged-on users | LSASS access by non-system proc (Sysmon EID 10) | Credential Guard, RunAsPPL |
| **Pass-the-Hash** | reuses NTLM hash (no crack) | NTLM from workstations to odd hosts | disable NTLM, tiered admin, Protected Users |
| **Kerberoasting** | service-account TGS → offline crack | TGS-REQ burst, RC4 (0x17) from workstation | AES keys, **gMSA**, monitor 4769 |
| **AS-REP roasting** | hash of pre-auth-disabled accounts | AS-REQ w/o pre-auth (EID 4768) | require pre-auth |
| **Pass-the-Ticket** | injects TGT/TGS | ticket used from a different host | Credential Guard, short lifetimes |
| **Golden Ticket** | forged TGT (krbtgt hash) | anomalous lifetimes, forged users | rotate krbtgt **twice**, Defender for Identity |
| **DCSync** | replicates password data | `DsGetNCChanges` (EID 4662) from non-DC | restrict replication rights |
| **ADCS ESC1** | cert for any UPN → persistent | cert request w/ odd SAN (EID 4886/4887) | fix template, CA audit |

**Detection blind spots** worth knowing as both attacker and defender: **LOLBins** (signed OS binaries — `certutil`, `regsvr32`/Squiblydoo, `mshta`, `wmic`, `rundll32`, `bitsadmin`, `netsh portproxy` — no malware signature, caught only by parent-child + network behavior); Kerberoasting that looks like service discovery; DCSync that looks like DC replication; long-dwell APTs that blend into a baseline; and **domain-fronted C2** over 443 to allowlisted CDNs (caught by JA3 fingerprinting + beacon-timing analysis). Map all of it to **MITRE ATT&CK** technique IDs.

## 5. Tooling

Kali/ParrotOS as the platform. Core kit: **nmap** (scanning), **Burp Suite** (web), **Metasploit** (exploitation framework), **BloodHound** + **impacket** + **Rubeus** + **Mimikatz** (AD), **CrackMapExec**/NetExec (lateral), **hashcat**/John (cracking), **Cobalt Strike**/Sliver (C2), **Responder** (LLMNR/NTLM capture), aircrack-ng/hcxtools (Wi-Fi), `ffuf`/`gobuster` (fuzzing), Ghidra/IDA (RE).

## 6. Digital forensics & anti-forensics

The flip side red teams must understand. **Forensics** recovers what happened: disk artifacts (MFT, prefetch, event logs, registry hives), **memory** (Volatility), and network (PCAP) — preserving a chain of custody and image hashes ([[Security Operations & IR#3. Digital forensics & evidence|IR forensics]]). **Anti-forensics** (used by adversaries, studied by defenders): log clearing/tampering, timestomping, secure deletion, in-memory-only execution, and encryption — which is exactly why logs must be shipped **off-host** and why memory capture comes first.

## 7. A realistic path in (purple team)

**Purple teaming** fuses offense and defense — "understand how attacks work *so that* you can detect them" (offense informs defense, and vice-versa); it's in high demand and well paid. A realistic 24–36-month arc: **Foundation** (Linux, networking, programming, web) → **parallel red + blue** (OSCP for offense; CySA+ plus hands-on **SIEM labs** writing detections for attacks you can perform) → **red deepening** (HTB Pro Labs, **CRTO**/Cobalt Strike, C2 infra) → **blue deepening** (threat hunting, detection engineering, DFIR). The differentiator is the home lab: run attacks in your lab, then build the SIEM detection that catches them. Practice platforms: **Hack The Box**, **TryHackMe**, CTFs.

Related: [[06 Cybersecurity|domain overview]] · [[Threats & Malware#6. APTs & MITRE ATT&CK|MITRE ATT&CK]] · [[Internet & Application Security#OWASP Top 10|OWASP Top 10]] · [[Network Security]] · [[Security Operations & IR]] · [[OpSec & Physical]] · [[Cybersecurity Foundations#5. Careers & certifications|careers]]
