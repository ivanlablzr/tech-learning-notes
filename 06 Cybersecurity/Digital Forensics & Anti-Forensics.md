---
type: note
tags: [cybersecurity, forensics, dfir, careers, blueteam]
domains: [cybersecurity, os, networking]
maturity: growing
confidence: high
updated: 2026-08-05
---

> [!abstract] What this note is
> The **deep, canonical note** on digital forensics, incident response (DFIR) & anti-forensics — the *what, why, how it works*, the **artifacts and tools**, and **how to get into the field** (with the French judiciary/state routes: Gendarmerie, Police, ANSSI, Armées). Shallow mentions elsewhere ([[Tooling, Forensics & Careers#2. Digital forensics & anti-forensics]], [[Security Operations & IR]]) now point here.

> [!info] Why this fits your profile
> Networks + cybersecurity + Linux/Windows interest **is already the base of a forensic analyst.** Forensics is the *defensive mirror* of the offensive work you're learning in [[Ethical Hacking|CPTS]]: the attacker hides, the analyst reconstructs. Learning both makes you rare — and it's a real second career door (state judiciary) if the private pentest path ever stalls. See [[IT & Cyber Job Market — Skills Employers Want]].

---

## 1. The five views

**Why it exists:** when a system is compromised, someone must answer *what happened, how, when, by whom, and what was taken* — reliably enough to drive remediation **and** stand up in court.

**Problem it solves:** turns the traces a system unavoidably leaves (files, logs, memory, packets) into a defensible **timeline of events** and a set of **IOCs** (indicators of compromise).

**Depends on:** deep **OS internals** ([[Windows_OS_and_Internals|Windows]], [[Linux_OS_and_Internals|Linux]]), **[[Memory|memory]]** architecture, **[[05 Networking|networking]]**, and **scripting** (Python/PowerShell/Bash) to automate analysis.

**Depended on by:** incident response, threat intelligence, legal/judicial process, cyber-insurance, and — offensively — **anti-forensics** (knowing what to erase because you know what's collected).

**The three sub-disciplines — don't conflate them:**

| Field | Question | Time pressure |
|---|---|---|
| **Digital Forensics (DF)** | *What exactly happened?* — rigorous, court-grade reconstruction | Low, methodical |
| **Incident Response (IR)** | *Make it stop, now* — contain, eradicate, recover | High, live |
| **Anti-forensics** | *Defeat/mislead the above* — attacker tradecraft | Adversarial |

DFIR = the combined practice. **DF prizes rigor; IR prizes speed** — the tension between them is the core craft.

---

## 2. The golden principles (get these wrong and the evidence is worthless)

1. **Order of volatility** — collect the most fleeting first: **RAM → network state → disk → backups**. Memory dies at power-off; a pulled plug destroys the best evidence of a fileless attack.
2. **Chain of custody** — every handling of evidence logged (who, when, what). Break it and the evidence is legally dead.
3. **Work on copies, prove integrity** — image the disk/memory, **hash it (SHA-256)**, analyse the *copy*, and re-hash to prove nothing changed. Write-blockers on originals.
4. **Reproducibility** — another analyst following your report must reach the same conclusion. This is why the **report** matters as much as the analysis (§7).
5. **Locard's principle, digitised** — every interaction leaves a trace. The attacker's job is to minimise it; yours is to find what they missed.

---

## 3. What you must understand — the artifact map

Forensics is *vast*. It's really **"where does each OS/layer record evidence, and how do I read it."**

### Windows (the richest artifact surface)

| Artifact | What it proves |
|---|---|
| **MFT** ($MFT) | Every file's metadata, even deleted — the master timeline source |
| **USN Journal** ($J) | File change log — creation/deletion/rename history |
| **Registry hives** (SYSTEM, SOFTWARE, NTUSER.DAT) | Config, autoruns, user activity, USB history |
| **Event Logs** (.evtx: Security, System, Sysmon) | Logons, process creation, service installs |
| **Prefetch** | *What executed and when* — key for malware |
| **Amcache / Shimcache** | Program execution & presence evidence |
| **SRUM** | App/network resource usage per process |
| **Volume Shadow Copies (VSS)** | Historical snapshots — recover "deleted" state |
| **Jump Lists / LNK / Recent** | Files a user opened |

→ builds on [[Windows]] and [[Windows_OS_and_Internals]].

### Linux

`ext4`/xfs internals · `systemd` & **`journalctl`** · **`auditd`** · `~/.bash_history` · `cron`/timers · `/var/log/*` (auth, syslog) · permissions & SUID · **LVM** snapshots · `/proc` live state. → builds on [[Linux]] and [[Linux_OS_and_Internals]].

### Memory (RAM) — the highest-value modern evidence

Acquire RAM **first**. Then hunt: hidden/injected processes, **DLL injection**, handles, mutexes, open sockets, unpacked malware, encryption keys, cleartext credentials. Tool: **Volatility 3**. Ties to [[Memory]].

### Network

TCP/IP · DNS · DHCP · VPN · **PCAP** analysis (**Wireshark**), **NetFlow**, **Zeek**, **Suricata** — find the attacker IP, C2 domain, exfil, downloaded payloads. Ties to [[05 Networking]].

### Mobile

Android & iOS · logical vs **physical** extraction · **SQLite** DBs (messaging apps) · SMS/photos/**geolocation**. (Commercial: Cellebrite/GrayKey.)

### Malware

**Static** (strings, PE headers, disassembly) vs **dynamic** (sandbox detonation) vs **reverse engineering** (Ghidra/IDA). Overlaps your [[Threats & Malware]] and the offensive [[Tooling, Forensics & Careers|RE tooling]].

### Cloud (now indispensable)

**AWS CloudTrail · Azure/Entra sign-in & audit logs · Microsoft 365 UAL · Google Workspace audit.** Modern breaches are increasingly identity-and-cloud, not just disk.

---

## 4. The essential toolkit

| Stage | Tools |
|---|---|
| **Acquisition** | FTK Imager, `dd`/`dc3dd`, **KAPE** (targeted triage), **Velociraptor** (fleet/live) |
| **Disk analysis** | **Autopsy** / **The Sleuth Kit**, X-Ways (commercial) |
| **Memory** | **Volatility 3**, MemProcFS |
| **Windows triage** | **Eric Zimmerman's tools** (MFTECmd, Registry Explorer, etc.) ⭐ |
| **Log/EVTX hunting** | **Chainsaw**, **Hayabusa** (Sigma rules) |
| **Timeline** | **Plaso/log2timeline** → **Timesketch** |
| **Network** | **Wireshark**, Zeek, NetworkMiner, Suricata |
| **Malware** | Ghidra, x64dbg, ANY.RUN / Cuckoo, YARA |

> **Learn the free stack deeply** (Autopsy, Volatility, Velociraptor, KAPE, Zimmerman, Wireshark, Chainsaw/Hayabusa) — it's what the state labs and most CERTs actually use, and it costs nothing to master in a home lab.

---

## 5. Anti-forensics — the offensive mirror (your CPTS edge)

What attackers do to defeat §2–4 — which is exactly *why* defenders collect the way they do:

| Technique | Counter (why defenders do X) |
|---|---|
| **Log clearing / tampering** | Ship logs **off-host** in real time (SIEM) — can't erase what already left |
| **Timestomping** (fake `$STANDARD_INFO` timestamps) | Cross-check `$FILE_NAME` timestamps in the MFT — attackers rarely fake both |
| **Fileless / memory-only** (LOLBins, reflective loading) | **Acquire RAM first** — nothing on disk to find |
| **Secure deletion / wiping** | Volume Shadow Copies, journal remnants, backups |
| **Encryption / packing** | Pull keys from memory; behavioural detection |
| **Living-off-the-land** ([[Credential Playbook|LOLBAS]]) | Baseline normal, hunt anomalies not signatures |

This section is where your offensive track pays a **direct dividend**: every anti-forensic trick in your [[Technique Catalog]] is something a forensic analyst must anticipate. You already study half of this from the attacker's side.

---

## 6. How to get into the field — France (state & judiciary)

If your goal shifts toward **incident response, digital forensics, and judicial investigation**, the routes differ sharply from private-sector pentest. *(Org names verified Aug 2026; the state is actively reorganising its cyber units.)*

### Gendarmerie nationale — probably the best fit for a systems/network profile

| Unit | Role | Level |
|---|---|---|
| **N-TECH** (enquêteurs en technologies numériques) | Seizure, evidence extraction, phone/disk/network investigation — present in many brigades | Entry into cyber-investigation |
| **C3N** (Centre de lutte contre les criminalités numériques, under **PJGN**) | The gendarmerie's cyber elite: ransomware, botnets, darknet, crypto, malware, international ops | Specialist |
| **IRCGN** (Institut de Recherche Criminelle, Pontoise) ⭐ | *The* forensic lab — disk/memory/mobile/GPS/video forensics, data recovery, cloud, timeline reconstruction; **builds its own tools** | The dream lab for forensics |
| **ComCyberGend → now "UNCyber"** | Command coordinating all the above (renamed 2025) | Umbrella |

### Police nationale

- **OFAC** (Office anti-cybercriminalité, created 2023, scaling to ~165 by 2027) — ransomware, phishing, compromises, international cooperation.
- **SRPJ** (regional judicial police) — computer investigations, seizures, evidence extraction.
- **Laboratoires de police scientifique** — phone/computer/server analysis, data recovery, malware.
- Entry as: *gardien de la paix*, *officier*, *commissaire*, **personnel scientifique**, or **contractuel spécialisé cyber**.

### Armées & ANSSI

- **COMCYBER** (military cyber command) & military intelligence — compromise investigation, more *national-defence* than judicial.
- **ANSSI** — not judicial, but one of the **best technical IR/forensic/malware environments in France**. CERT-FR does real incident response.

### Private sector (common on-ramp, many later join the state)

CERT / **CSIRT** teams & incident-response firms: **Orange Cyberdefense, Sopra Steria, Wavestone, Capgemini, Synacktiv**. A private DFIR role builds the exact portfolio the state values.

> [!warning] Honest note on the state route
> Police/Gendarmerie access usually runs through a **concours**, military recruitment, or a specific status (scientifique/contractuel): written + oral exams, **enquête administrative** (background check), medical, sometimes physical tests, then initial training *before* any cyber specialisation. Direct-entry expert/contractuel posts exist for strong technical profiles but are competitive. **Plan 12–18 months and a real portfolio**, not a quick pivot.

---

## 7. The report — the skill that actually differentiates

Investigators spend enormous time **writing**. A forensic report must be:

- **Objective** — findings, not speculation; separate fact from inference.
- **Reproducible** — tools, versions, hashes, steps, so anyone can re-verify.
- **Chronological** — a clear timeline of events.
- **Legally exploitable** — chain of custody intact, defensible in court.

> This is where your existing **writeup habit** ([[Cyber Events, CTFs & Community]]) and your [[Decorec|pentest reporting]] transfer *directly*. A portfolio of clean investigation reports is worth more than any single certificate to IRCGN/C3N.

---

## 8. Certifications (ordered) & the cost reality

| Stage | Certs | Note |
|---|---|---|
| **Foundations** | Google Cybersecurity → CompTIA **Security+** → **CySA+** | Cheap, HR-filter, blue-team baseline |
| **DFIR flagship** | **GCFA** (forensic analyst), **GCFE** (examiner) | SANS FOR500/FOR508 — excellent, **~$9k each** |
| **Malware/RE** | **GREM** | SANS FOR610 |
| **Mobile** | **Cellebrite (CCO/CCPA)** | Only if an employer funds it |
| **Free/cheap practical** | HTB **CDSA**, TryHackMe DFIR paths, BlueTeamLabs | Best value to *start* |

> [!danger] Don't self-fund SANS/GIAC
> GCFA/GCFE/GREM are the gold standard **and ~$9,000 all-in each** — priced for employer budgets. Get hired (private CERT, ANSSI, or a gendarmerie/police cyber post) *then* have them funded. Self-fund only the cheap practical rung (CDSA, THM). Full pricing in [[IT Certifications and Learning Resources per Domain]].

---

## 9. Projects — what actually builds the portfolio

The differentiator, per every recruiter in this space. Do these in your [[16 Home Lab Projects|home lab]] and **publish the reports**:

1. **Forensic lab** — image an old disk, analyse it, write a full investigation report.
2. **Windows intrusion** — detonate a benign test sample in an isolated VM → RAM + disk acquisition → timeline (Plaso/Timesketch) → IOCs → report.
3. **Linux intrusion** — same drill on Linux artifacts (auditd, journal, bash history).
4. **Network case** — analyse a PCAP: find attacker IP, C2 domain, downloaded files, exfil.
5. **Mobile** — Android backup → extract SMS/photos/geolocation/SQLite.
6. **DFIR CTFs** — CyberDefenders, BlueTeamLabs, [[Cyber Events, CTFs & Community|forensic CTFs]] — measurable, public proof.

Public datasets to practise on: **DFIR.training**, **Digital Corpora**, **CyberDefenders**, SANS DFIR challenges.

---

## 10. The profile that stands out (for C3N / IRCGN / police labs)

- Excellent **Windows + Linux internals** (filesystems, logs, artifacts).
- Solid **networks & protocols**.
- A **GitHub/Obsidian portfolio** documenting *complete, methodical* investigations.
- Investigation reports written like real **expertise reports**.
- Hands-on **Autopsy, Volatility, Velociraptor, KAPE, Wireshark**.
- **Scripting** (Python/PowerShell/Bash) to automate analysis.
- Regular **DFIR CTFs** and technical watch.

> [!tip] Where this sits vs your main path
> Your spine is **offensive** (CPTS → OSCP → red team). DFIR is a **strong adjacent option**, not a competing one: the OS/network internals overlap ~70%, and anti-forensics literally *is* offensive knowledge. Treat this as a door you keep open — especially the **Gendarmerie/IRCGN** route, which suits your systems+network base and rewards a documented portfolio over paper. Decide deliberately; don't split daily focus while CPTS is unfinished ([[Mission Priority]]).

---

## Connections

- [[Security Operations & IR]] — the IR/SOC side; forensics is its evidence engine
- [[Tooling, Forensics & Careers]] — the offensive toolkit & the anti-forensics mirror
- [[Windows_OS_and_Internals]] · [[Linux_OS_and_Internals]] · [[Memory]] — the internals forensics reads
- [[Threats & Malware]] — malware analysis overlap
- [[Credential Playbook]] · [[Technique Catalog]] — the attacker tradecraft you learn to detect
- [[IT & Cyber Job Market — Skills Employers Want]] — where DFIR sits in the market
- [[IT Certifications and Learning Resources per Domain]] · [[Cyber Events, CTFs & Community]] — certs & DFIR CTFs
- [[16 Home Lab Projects]] — where the §9 projects run
- [[Financial Privacy & Financial Crime]] — the money-crime investigation crossover

## Related

[[06 Cybersecurity]] · [[Cybersecurity Skills Roadmap]] · [[Master Index — Technology Vault]]
