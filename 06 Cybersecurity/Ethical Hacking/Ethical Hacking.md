---
type: note
tags: [cybersecurity, redteam, pentesting, offensive]
---


The offensive reference — **authorized** attack simulation that finds weaknesses before real adversaries do. Everything here assumes written permission; without it, the same actions are crimes (see [[Tooling, Forensics & Careers#Laws & regulations|laws]]).

> [!info] This topic is split across five notes. Start here, then follow the map below.

## The big picture

Ethical hacking is a **repeatable process**, not a bag of tricks. You move outside-in: agree on the rules, learn the target, find a way in, expand control, prove impact, then write it all up so the client can fix it. The same kill-chain applies whether the target is a web app, a Windows domain, a phone, or a building.

Two ideas to anchor everything:

- **Offense informs defense.** Every attack technique has a matching detection. The most valuable practitioners (purple teams) can both run an attack *and* build the alert that catches it.
- **The report is the product.** Clients remember how clearly you explained the risk, not how clever the exploit was. A finding nobody can act on is worthless.

## The five notes

| Note | What it covers |
|---|---|
| [[Engagement & Methodology]] | The legal setup (RoE, engagement types) and the 8-phase methodology from recon to close-out |
| [[Technique Catalog]] | Attack techniques organized by surface — web, OS, Active Directory, mobile, hardware, RF, physical |
| [[Credential Playbook]] | The Windows/AD path from foothold to Domain Admin, and how each step is detected |
| [[Tooling, Forensics & Careers]] | The toolkit, digital forensics & anti-forensics, the purple-team career path, and the law |

## How the phases connect

```text
Pre-engagement → Recon → Vuln assessment → Exploit → Post-exploit
       (contract)   (learn)    (map & probe)   (get in)   (dig in)
                                                              │
                              Report ← PoC ← Lateral movement ┘
                          (prove it)        (spread)
```

Each phase feeds the next: recon tells you what to scan, scanning tells you what to exploit, a foothold enables post-exploitation and lateral movement, and everything you do becomes evidence in the report. The loop often repeats — every new host you land on restarts at "learn this machine."

Related: [[06 Cybersecurity|domain overview]] · [[Threats & Malware#6. APTs & MITRE ATT&CK|MITRE ATT&CK]] · [[Internet & Application Security#OWASP Top 10|OWASP Top 10]] · [[Network Security]] · [[Security Operations & IR]] · [[OpSec & Physical]]
