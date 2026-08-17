#cybersecurity #roadmap #redteam #skills #certs #projects


> **The one note.** For every topic: **🎯 what to learn (concepts)** · **📜 what validates it (certs)** · **🛠️ what proves it (a project → GitHub writeup).** Ordered **red-team-first** and by **dependency** — you can't attack Active Directory before you understand networking and Windows.
> **How to use it:** go **top-to-bottom, one domain at a time**, finish each with its project, and each Sunday pull the next topic into [[Mission Priority]]. This note is what you *learn from*; the rest just feed it → capstone build detail: [[Home Lab]] · broad multi-domain certs (networking/cloud/AI): [[IT Certifications and Learning Resources per Domain]] · jobs: [[Cyber Career — Field Paths & Alternance (France)]] · market data: [[Cyber Career — Roles, Skills & Certs (Market Research)]].
> ⚠️ *Anti-overwhelm ([[Personal weaknesses]]): depth-first in order, not breadth-first. One domain → one project → next. Perfecting the roadmap is not studying.*
>
> **Legend:** ⭐ = your priority / suggested order · 🎯 concepts · 📜 certs · 🛠️ project.

## 🟢 START HERE (your position)
You're **mid-CPTS** = across **Tier 0 → Tier 1**. Do this now:
1. Make **Linux + Networking** automatic (Tier 0.1–0.2) — everything rests on them.
2. Run the **methodology + enumeration** on HTB boxes (Tier 1).
3. **Publish one box writeup** (scope → enum → exploit → privesc → remediation). One great writeup > five half-done boxes. *That single act starts the portfolio your whole plan depends on.*

## 🗺️ The fields & which tiers they need
So you see "the different fields" and their priority. **Red team = your spine (Tiers 0→2)**; the others branch at Tier 3.

| Field | T0 Foundations | T1 Core offensive | T2 Red team | T3 branch |
|---|:--:|:--:|:--:|---|
| **Pentest / Red Team** ⭐ | ✅ | ✅ | ✅ | cloud/AD/OT as needed |
| DFIR / IR *(your #2)* | ✅ | ✅ | know the attacks | ➕ forensics + hunting |
| SOC / Blue | ✅ | ✅ *(defensive lens)* | — | ➕ detection/SIEM |
| Cloud Security | ✅ | ✅ | partial | ➕ cloud (core) |
| AppSec / Web | ✅ | ✅ | partial | ➕ advanced web + code |
| GRC / Audit | ✅ *(lighter)* | concepts | — | ➕ compliance |

## 📜 Cyber certification ladder at a glance (detail lives per-domain below)
- **Offensive spine ⭐:** HTB **CPTS** *(now)* → HTB **CBBH** / TCM **PNPT** → **OSCP** → **CRTO** (AD) → **OSWE** (web) / **OSEP** (evasion) → later **CRTL / OSCE³ / GXPN**.
- **Blue / SOC:** Security+ → CySA+ / **BTL1** → GCIH / GCIA.
- **DFIR:** **GCFA** → GCFE / GNFA / CHFI / BTL2.
- **Cloud:** AZ-500 / AWS Security Specialty / **CARTP** (cloud red team) / CKS (k8s).
- **GRC:** ISO 27001 LA → **CISSP** (needs ~5 yrs) / CISM.
- *Rule you set: certs pass HR filters, **projects prove skill**. Take the cert only after the matching skill is real.*

---

# 🧱 Tier 0 — Foundations (non-negotiable base)

### 0.1 Linux
🎯 **Concepts:** CLI navigation & pipes/redirection, filesystem hierarchy, users/groups & **permissions (rwx, SUID/SGID, sudo)**, processes & signals, **systemd/services**, package managers, cron, networking commands (`ip`, `ss`, `dig`), log locations (`/var/log`, journald), text tools (grep/sed/awk), **bash scripting**.
📜 **Certs:** LPIC-1 / Linux+ *(optional)* · RHCSA *(if sysadmin-leaning)*.
🛠️ **Project:** harden a fresh Ubuntu/Debian server from a written checklist; document every change and *why*. → [[Linux]], [[Shells & Command Line]].

### 0.2 Networking
🎯 **Concepts:** OSI & TCP/IP models, **IP addressing & subnetting/CIDR**, [[Ports, Interfaces & Sockets|ports · sockets · states]], **TCP handshake / UDP**, DNS · DHCP · HTTP/S · **TLS**, ARP, routing & switching, **VLANs**, **NAT/PAT**, **firewalls & ACLs**, proxies, VPNs, Wireshark/tcpdump packet reading.
📜 **Certs:** CompTIA **Network+** · (CCNA if you want the depth) — full ladder in [[IT Certifications and Learning Resources per Domain]].
🛠️ **Project:** build a **segmented network** (pfSense/OPNsense firewall + VLANs) and *prove* isolation with packet captures. → [[05 Networking]], [[OSI Layers & Protocols]], [[Home Lab]] Phase 0.

### 0.3 Windows & Active Directory basics
🎯 **Concepts:** Windows internals (processes, handles, tokens), **registry**, services, event logs, **SMB/CIFS**, users/groups/ACLs, UAC, **PowerShell**, and *what AD is* — domains, forests, OUs, GPOs, **Kerberos/NTLM**, LDAP (attacks come in Tier 2).
📜 **Certs:** MS **AZ-104** / MD-102 *(optional, sysadmin path)*.
🛠️ **Project:** stand up a **Windows Server domain controller + 2 clients**, apply a GPO, create users. → [[Windows]], [[Shells & Command Line]], [[Home Lab]] Phase 1.

### 0.4 Programming & scripting
🎯 **Concepts:** **Python** (data types, functions, files, `requests`, sockets, argparse, virtualenvs), **Bash**, basic **PowerShell**, reading others' code/exploits, HTTP programmatically, JSON/regex parsing, Git & GitHub.
📜 **Certs:** PCEP/PCAP (Python) *(optional)*.
🛠️ **Project:** a **Python asset/port scanner** that nmaps your lab and writes findings to a file/DB. → [[07 Programming]].

### 0.5 Virtualization & the lab
🎯 **Concepts:** hypervisors (Proxmox/VMware/VirtualBox), **snapshots/clones**, VM networking (bridged/NAT/host-only), **Docker** basics, resource sizing, isolation.
🛠️ **Project:** the lab exists, snapshots work, red-team VLAN is isolated. → [[Home Lab]] Phase 0.

### 0.6 Security fundamentals (the theory under everything)
🎯 **Concepts:** **CIA triad**, **AAA** (authn/authz/accounting), risk = threat × vuln × impact, **threat modeling (STRIDE)**, defense-in-depth, least privilege, **cryptography basics** (symmetric AES, asymmetric RSA/ECC, hashing, PKI/certs), the **kill chain** & **MITRE ATT&CK**.
📜 **Certs:** **CompTIA Security+** *(the universal baseline — worth it even for red team)*.
🛠️ **Project:** a **threat model (STRIDE) of your lab** — one page, data flows + top risks. → [[Cybersecurity Foundations]], [[Cryptography]].

---

# ⚔️ Tier 1 — Core offensive (the pentest engine = CPTS)

### 1.1 Methodology & the kill chain
🎯 **Concepts:** pre-engagement/**RoE**, recon → enumeration → vuln assessment → exploitation → **privesc** → post-exploitation → **lateral movement** → **reporting**; note-taking discipline. → [[Hacking Engagement & Methodology]].

### 1.2 Reconnaissance & enumeration *(the phase that wins engagements)*
🎯 **Concepts:** passive OSINT (Google dorking, certificate transparency, `whois`, Shodan), active scanning — **nmap deeply** (scan types, `-sV`, NSE, timing/evasion), service enumeration (SMB, LDAP, SNMP, HTTP, DNS), **directory/vhost brute-forcing** (ffuf/gobuster), banner grabbing.
🛠️ **Project:** a full **enumeration writeup** of one HTB box — every service, every finding.

### 1.3 Vulnerability identification & exploitation
🎯 **Concepts:** mapping service→CVE, **Metasploit** *and* manual exploitation, understanding what an exploit does, public exploit modification, buffer-overflow *awareness* (deep in OSED later).
🛠️ **Project:** exploit 3 different services manually (no one-click), documented.

### 1.4 Shells & payloads
🎯 **Concepts:** **bind vs reverse vs web shells**, listeners (`nc`, socat, msfconsole), staged/stageless payloads, msfvenom, **TTY upgrade** to a full interactive shell. → [[Shells & Payloads]].
🛠️ **Project:** catch a reverse shell in your lab and fully upgrade the TTY; write up the difference.

### 1.5 Privilege escalation (Linux + Windows) *(huge, high-value)*
🎯 **Concepts — Linux:** SUID/SGID, sudo misconfig, cron, capabilities, PATH abuse, kernel exploits, `GTFOBins`. **Windows:** token impersonation, service/registry misconfig, unquoted paths, **AlwaysInstallElevated**, `LOLBAS`, DLL hijacking, credential harvesting.
📜 **Certs (covering Tier 1 as a whole):** ⭐ **HTB CPTS** *(you)*, TCM **PNPT/PJPT**, INE **eJPT**.
🛠️ **Project:** a **privesc cheat-writeup** — 3 Linux + 3 Windows techniques demonstrated in your lab.

### 1.6 Password & credential attacks
🎯 **Concepts:** hashing vs encoding, **Hashcat/John**, wordlists & rules, password **spraying** vs brute-force, **pass-the-hash**, credential locations (SAM, LSASS, `/etc/shadow`, browsers). → [[Credential Playbook]].
🛠️ **Project:** crack a set of hashes and document methodology + defenses.

### 1.7 Web application security
🎯 **Concepts:** **OWASP Top 10** (injection/SQLi, **XSS**, broken auth, **IDOR**, SSRF, XXE, deserialization, misconfig, SSTI), **Burp Suite** (proxy/repeater/intruder), auth/session testing, API testing. → [[Internet & Application Security]].
📜 **Certs:** HTB **CBBH**, **BSCP** (Burp), eWPT.
🛠️ **Project:** a **web-app pentest writeup** of **OWASP Juice Shop / DVWA** — findings, PoC, remediation.

### 1.8 Report writing *(what actually gets you hired)*
🎯 **Concepts:** exec summary vs technical detail, scope, risk rating (CVSS), reproduction steps, business impact, remediation, writing for a non-technical owner.
🛠️ **Project:** turn any box into a **client-grade report** (not just a walkthrough).

---

# 🥷 Tier 2 — Red-team specialization (your differentiator)

### 2.1 Active Directory attacks *(the #1 enterprise surface)*
🎯 **Concepts:** enumeration (**BloodHound/SharpHound**, ldapdomaindump), **Kerberoasting / AS-REP roasting**, **NTLM relay**, ACL/ACE abuse, delegation (unconstrained/constrained/RBCD), **lateral movement** (PsExec/WMI/WinRM), **DCSync**, **Golden/Silver tickets**, domain trusts.
📜 **Certs:** ⭐ **CRTO** (Zero-Point) · CRTP → CRTE (Altered Security).
🛠️ **Project:** own **GOAD** (Game of Active Directory) foothold→**Domain Admin**, full attack-path writeup.

### 2.2 C2 & post-exploitation
🎯 **Concepts:** command-and-control (**Sliver**, Havoc, Cobalt Strike concepts), beaconing, **persistence** (scheduled tasks, services, registry, WMI), situational awareness, data exfil, opsec of the operator.
🛠️ **Project:** set up a C2, get a beacon in the lab, establish persistence, document it.

### 2.3 Pivoting & tunneling
🎯 **Concepts:** reaching segmented nets through a foothold — SOCKS proxies, **port-forwarding** (SSH, chisel, ligolo-ng), double-pivots. Ties to [[Ports, Interfaces & Sockets]].
🛠️ **Project:** pivot from a DMZ host into an internal VLAN in your lab; diagram the path.

### 2.4 Evasion, OPSEC & AV/EDR bypass (intro)
🎯 **Concepts:** how **AV/EDR/AMSI** detect, obfuscation, LOLBins, in-memory execution, egress considerations, **offense informs defense**.
📜 **Certs:** ⭐ **OSCP/OSCP+** (the anchor — spans Tier 1–2) → **OSEP** (evasion).
🛠️ **Project:** a **purple-team writeup** — run an attack, then catch it in your own SIEM ([[Home Lab]] Phase 2).

### 2.5 Advanced web
🎯 **Concepts:** auth bypass chains, **deserialization RCE**, SSRF→cloud metadata, request smuggling, prototype pollution, JWT attacks, GraphQL.
📜 **Certs:** ⭐ **OSWE** · eWPTX.
🛠️ **Project:** exploit a deserialization or SSRF chain in a deliberately-vulnerable app; write it up.

### 2.6 Wireless & RF *(your interest)*
🎯 **Concepts:** WPA2/WPA3 attacks, Evil Twin, handshake capture/cracking, Bluetooth/BLE, **RFID/NFC**, sub-GHz (Flipper). Ties to [[Network Media & Links]], [[Ports, Interfaces & Sockets]].
🛠️ **Project:** attack your *own* test AP (handshake→crack); document defenses.

---

# 🌩️ Tier 3 — Field branches (pick by role/interest; not all needed)

### Cloud security & cloud pentest
🎯 **Concepts:** **Entra ID/Azure**, AWS, GCP; **IAM** & privilege escalation paths, misconfigured storage, metadata SSRF, **CSPM**, secrets, hybrid AD. 📜 **AZ-500 · AWS Security Specialty · CARTP.** 🛠️ deploy lab to a cloud free-tier via Terraform, **find & fix an over-permissive IAM role** → [[Home Lab]] Phases 4–5, [[09 Cloud]].

### Containers & Kubernetes security
🎯 **Concepts:** image/registry security, container escapes, **K8s RBAC**, admission control, secrets. 📜 **CKS.** 🛠️ break out of a misconfigured container; harden a k3s cluster → [[10 DevOps]].

### DevSecOps & automation
🎯 **Concepts:** **Terraform/Ansible** (IaC), CI/CD security scanning (Trivy, SAST/DAST), Git security, secrets management, **Python tooling**. 🛠️ rebuild the whole lab from code; add a security gate to a pipeline.

### DFIR, threat hunting & detection engineering *(your #2 path)*
🎯 **Concepts:** **memory forensics (Volatility)**, disk/timeline (Autopsy/KAPE), Windows artifacts, log analysis, **SIEM (Wazuh/Elastic/Splunk)**, **Sigma rules**, MITRE ATT&CK mapping, IR playbooks. 📜 **GCFA · GCIH · BTL1/2.** 🛠️ build a **home SOC** (Wazuh + Sysmon), catch your own attack, write the **incident report** → [[Home Lab]] Phase 2; [[Cyber Career — Field Paths & Alternance (France)|Field Paths]] top pick.

### Malware analysis & reverse engineering
🎯 **Concepts:** static vs dynamic analysis, PE format, **Ghidra/IDA**, sandboxing, unpacking, x86/x64 assembly basics, YARA. 📜 **GREM.** 🛠️ analyse a benign/sample binary in an isolated VM; write a report.

### Exploit development (advanced offensive)
🎯 **Concepts:** stack/heap overflows, shellcoding, ASLR/DEP bypass, fuzzing. 📜 **OSED (→ OSCE³).** 🛠️ write a working exploit for a known-vulnerable app.

### OT/ICS & hardware hacking *(your electronics interest)*
🎯 **Concepts:** PLCs/SCADA, Modbus, **serial/UART** console→root, JTAG, firmware extraction, RFID. 🛠️ get a **UART shell** on a router; firmware analysis writeup → [[Home Lab]] Phase 7, [[Digital Logic & Microcontrollers]].

### GRC, audit & compliance *(for "cyber jobs of all kind")*
🎯 **Concepts:** **ISO 27001** ISMS, risk assessment, **NIS2 · RGPD · SecNumCloud · DORA**, audit methodology, policy writing, **PASSI** context. 📜 **ISO 27001 LA · CISSP · CISA.** 🛠️ run an **ISO 27001 gap assessment** on your lab; write the audit report.

### Mobile security *(lower priority)*
🎯 Android/iOS app testing, MobSF, Frida. 🛠️ pentest a deliberately-vulnerable mobile app.

---

# 🔁 Cross-cutting mastery (from day 1)
- **Report writing & communication** — the hire-maker. **Methodology & note discipline** — structured engagement notes. **Portfolio** — *every project → a GitHub writeup* (your proof, per your projects-over-certs rule). **OSINT**. **English + client-facing soft skills** ([[Entrepreneuriat|selling/consulting]]). **CTFs** (HTB, TryHackMe, live events) as continuous practice.

# 🛠️ The projects — turning learning into experience
**Many small projects, each proving one skill, all built inside ONE growing lab → they compound into the capstone.** Each = a GitHub writeup (a portfolio piece). Beats one monolith and fits your overwhelm tendency.

**The proof-project ladder (each a writeup):**
1. Harden a Linux server · 2. Segmented network + firewall · 3. Python port/asset scanner · 4. Stand up AD domain · 5. Threat-model your lab · 6. 8–10 HTB boxes · 7. Web pentest (Juice Shop) · 8. Own **GOAD** to Domain Admin · 9. Home **SOC + purple-team** report · 10. Cloud deploy + IAM misconfig fix · 11. Malware-analysis writeup · 12. UART/hardware hack.

**Capstone = your whole [[Home Lab]] "Acme Corp"** — the enterprise you build, attack, detect, and report on end-to-end. Projects 1–12 *are* its phases. That one documented lab is the strongest thing you can show a French/Swiss employer or client.

# 🔗 Sequencing with your system
- **Each Sunday** ([[Mission Priority]] review): pull the *next topic in the current tier* into This Week — never jump tiers.
- **Certs** come *after* the matching skills are real (see the per-domain 📜).
- **Jobs:** once Tier 1 + 3–4 writeups exist → start applying via [[Cyber Career — Field Paths & Alternance (France)]].
- **Minimum-day still counts.** Depth over breadth. Ship one writeup at a time.

## Connects to
[[06 Cybersecurity]] · [[Ethical Hacking]] · [[Home Lab]] · [[Mission Priority]] · [[IT Certifications and Learning Resources per Domain]] · [[Cyber Career — Roles, Skills & Certs (Market Research)]] · [[Cyber Career — Field Paths & Alternance (France)]] · [[05 Networking]] · [[Linux]] · [[Windows]] · [[07 Programming]] · [[Ports, Interfaces & Sockets]] · [[Shells & Payloads]]
