---
type: note
tags: [cybersecurity, pentesting, methodology, redteam]
---


How a real engagement runs — first the legal/contractual setup, then the repeatable kill-chain from reconnaissance to close-out.

## 1. Engagement basics

Everything starts with **written authorization**. The **Rules of Engagement (RoE)** is the document that defines the legal boundary, and it must cover:

- **Scope** — exactly which IP ranges, domains, cloud accounts, and physical sites are in or out (and confirm the client actually owns them; third-party/CDN assets need their provider's consent).
- **Time windows** — when testing is allowed, plus blackout periods.
- **Emergency contacts** — a "get-out-of-jail" call tree for when something breaks or you trip an alarm.
- **Data handling** — how findings and captured data are stored, encrypted, and destroyed.

### Types of engagement

| Engagement | Start position | Goal |
|---|---|---|
| **External pentest** | no creds, outside the network | initial access → escalate |
| **Internal / assumed breach** | low-privilege domain user | reach Domain Admin, exfil data |
| **Red team** (APT simulation) | zero knowledge | objective-based, evade detection |
| **Purple team** | coordinated with defenders | validate that attacks ↔ detections line up |

You also pick a **threat-actor persona** to imitate, and it sets your stealth posture: a nation-state actor is slow and quiet; a cybercriminal is fast and noisy. For stealthy work, red-team **OPSEC** means dedicated attack infrastructure (a VPS), command-and-control hidden behind redirectors so the real C2 IP is never exposed, aged/categorized domains, and out-of-band team comms.

Common frameworks that standardize all this: **PTES**, the **OWASP Testing Guide**, **NIST SP 800-115**, and **CREST**.

## 2. Pre-engagement (the paperwork)

Before any testing, a sequence of documents gets signed. In order:

1. **NDA** — signed right after first contact, before any details are discussed. Confirm *who* in the client org is actually authorized to commission a test, so you don't take on legal risk.
2. **Scoping questionnaire** — the client picks which services they want and supplies the facts you need to plan.
3. **Scoping document** — the agreed scope, written down during the pre-engagement meeting.
4. **Penetration testing proposal** (the contract / statement of work) — services, cost, and timeline.
5. **Rules of Engagement** — the operational boundary (see §1).
6. **Contractors agreement** — extra authorization for physical assessments.
7. **Reports** — produced during and after the test.

### NDA types

| Type | Who's bound | Typical use |
|---|---|---|
| **Unilateral** | one party only | the other side can share what it receives |
| **Bilateral** | both parties | the standard for pentests — protects the tester's work |
| **Multilateral** | three or more parties | needed when several orgs share one network |

### What the scoping questionnaire pins down

The questionnaire turns a vague request into a plannable job. It establishes the **assessment type** (internal/external pentest, web/app, wireless, physical, social-engineering, red team), the **knowledge level**, the **stealth level**, and the practical constraints — test locations (on-site vs. remote over VPN), third-party providers whose consent is required, compliance regimes that apply (HIPAA, PCI, etc.), accepted **risks** (log noise, possible account lockouts, performance impact), contacts, reporting format, and payment terms.

Two dials worth knowing by name:

- **Knowledge level** — **black box** (tester gets nothing), **grey box** (just IPs/URLs), or **white box** (full documentation).
- **Stealth level** — **non-evasive** (loud, full coverage), **fully evasive** (quiet, mimics a real attacker), or **hybrid-evasive** (start quiet, gradually get louder to measure *when* the client's team notices).

The signed **RoE** then records all the operational details in writing: named client and tester contacts, the exact in-scope IPs/domains/accounts, the testing window and blackout dates, the agreed knowledge and evasiveness levels, how sensitive findings and evidence are handled, and the authorizing signature from someone empowered to grant permission.

For physical assessments, a separate **contractors agreement** gives the testers explicit written authorization to be on the premises and attempt entry — the "get-out-of-jail" letter they carry to prove the activity is sanctioned if security or police stop them. It names the authorized individuals, the permitted locations and dates, and the allowed techniques (lock bypass, tailgating, badge cloning, etc.).

## 3. The methodology

![[Pasted image 20260613155848.png]]
Representation made by HackTheBox

A repeatable kill-chain, outside-in toward the objective. Each phase produces the inputs for the next.

When working in different domain (like a factory, company's network, hospital devices, etc) the techniques used throughout the steps of the hacking methodology will change. That's why learning new concepts like web exploitation, mobile exploitation, Active Directory environments, and many others are important because our skills should match the environments of the clients.

### 3.1 Reconnaissance — learn the target

Gather information while weighing four constants: risk of detection, data accuracy, legal/ethical limits, and time. Two modes:

**Passive** (no contact with the target) comes first. This is OSINT work — public websites, social media, WHOIS (domain ownership/IP), DNS enumeration (subdomains, mail servers), certificate-transparency logs (`crt.sh`, Amass, subfinder), `theHarvester`, Shodan for exposed assets, and GitHub dorking for leaked secrets (`trufflehog`, stray `.env`/`id_rsa`/API keys). LinkedIn and similar sources map employees for later social engineering. Tools like Maltego and Recon-ng aggregate it.

**Active** (direct interaction) is louder and can trip alerts, so it demands solid networking knowledge and care to stay inside scope. Core techniques: **port scanning** (find open services), **network mapping** (topology), **banner grabbing** (software versions/config), and **vulnerability scanning** (test for known flaws).

### 3.2 Vulnerability assessment — map and probe

Map live hosts, ports, services, and versions (`nmap`, masscan), then probe each service deeply: web content discovery (`ffuf`, gobuster), SMB/LDAP/SNMP enumeration, and automated vuln scanning (Nessus, nuclei). The output is a prioritized list of things worth attacking.

### 3.3 Exploitation — get in

Turn a weakness into access: web exploits (SQLi, malicious file upload, SSRF, deserialization), service CVEs (Metasploit, public PoCs), default/weak credentials, or phishing (GoPhish with a macro / HTML-smuggling / ISO-LNK payload).

Prioritize candidate attacks by **probability of success**, **potential damage**, and **complexity**. When a public proof-of-concept exists, use it; when it doesn't, rebuild the exploit on a local VM that mirrors the target so you can adapt it safely before firing at production.

### 3.4 Post-exploitation — dig in

Once you have a foothold, stabilize and expand it:

- **Get a stable shell** and gather local information.
- **Establish persistence** — a scheduled task, run key, or service that survives reboots.
- **Escalate privilege** — the goal is `root` (Linux) or `Administrator`/`SYSTEM` (Windows), which usually removes restrictions on movement. Linux: `sudo -l`/GTFOBins, SUID binaries, kernel exploits (e.g. PwnKit). Windows: token abuse, unquoted service paths, dumping LSASS with Mimikatz/procdump.
- **Pillage** for credentials and sensitive data, then **exfiltrate** — mindful of data-security regulations on what you're allowed to remove.
- **Stay evasive** — commands like `net user` or `whoami` are flagged by EDR, so a stealthy engagement avoids or disguises them.

When exfiltrating data as proof, act as a careful custodian: take the minimum needed to demonstrate impact, encrypt it in transit and at rest, log exactly what was taken, and destroy it after the engagement. Moving regulated data (PII, PHI, cardholder data) can itself break the law, so confirm the RoE explicitly permits it before touching it.

### 3.5 Lateral movement — spread

Use harvested credentials to pivot to new hosts: **Pass-the-Hash**, remote execution via **PsExec/WMI/WinRM**, and **BloodHound** to chart the shortest path to Domain Admin. Defenses you'll meet — segmentation, IDS/IPS, EDR, threat monitoring — only fall if you understand what each one keys on. Each new host restarts the inner loop: enumerate → assess → exploit → post-exploit. (Full credential techniques: [[Credential Playbook]].)

### 3.6 Proof of Concept

A script or piece of code that reliably triggers a vulnerability you found. Its job is to **prove the problem is real** so developers and admins can reproduce it, see the impact, and verify their fix.

### 3.7 Post-engagement — close out cleanly

The phase that separates a professional from a hobbyist:

- **Clean up.** Remove uploaded tools/scripts and revert changes. If you can't reach a system to undo something, tell the client and list it in the report appendix. Document every change you made (e.g. a local admin account you added) so the client can confirm later alerts were your sanctioned activity.
- **Report and review.** Deliver a clear report — findings, business risk, reproduction steps, prioritized remediation — then walk the client through it in a review meeting.
- **Handle the draft → final flow.** Reports ship marked `DRAFT`; after the client comments, you reissue a `FINAL`. Some audit firms reject anything still marked draft, so keep this consistent across clients.
- **Retest after remediation** to confirm fixes actually closed the holes. The retest repeats only the relevant tests against the fixed systems and produces a short delta report marking each finding as resolved, partially mitigated, or still open.
- **Stay impartial.** A pentest is an audit, so you advise on fixes but never implement them. Give general guidance ("sanitize user input") rather than rewritten code or live config changes — implementing fixes yourself creates a conflict of interest.
- **Retain data per policy.** Retention is set by contract and any compliance regime: keep the report and evidence encrypted for the agreed period (often until the client has remediated and been retested), then securely destroy them. Never hold client data longer than authorized.
- **Close the project.** Wipe or destroy any systems used to touch client data, securely store leftover artifacts, invoice, and follow up with a satisfaction survey — often the seed of repeat work.

> The client usually remembers the **communication and professionalism**, not the exploit chain. Soft skills make the consultant.

Related: [[Ethical Hacking|offensive overview]] · [[Technique Catalog]] · [[Credential Playbook]] · [[Tooling, Forensics & Careers]] · [[OpSec & Physical#Social engineering|social engineering]]
