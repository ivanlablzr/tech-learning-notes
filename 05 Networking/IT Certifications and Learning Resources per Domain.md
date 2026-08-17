---
type: note
tags: [networking, certifications, career, costs]
domains: [networking, cybersecurity, cloud, ai, business, os]
maturity: growing
updated: 2026-07-20
---

A curated roadmap of industry-recognized certifications organized by career domain. Focus on building strong fundamentals first, then progressing toward expert-level specialization.

> **Learning philosophy:** Theory → Hands-on labs → Projects → Certification → Real-world experience.
>
> ⭐ = **recommended for you** (red-team-first, given your projects-over-certs approach — see [[Cyber Career — Roles, Skills & Certs (Market Research)]]. A ⭐number = suggested order. **Always pair a cert with a project/writeup: the cert passes the HR filter, the project proves the skill.**

---

> [!warning] How to read the cost and time columns
> **Costs** are list price in USD unless noted, checked July 2026. From France/Switzerland, expect to add **VAT (20% FR / 8.1% CH)** and note that Pearson VUE and vendor portals bill in local currency at their own rate — budget ~10–20% above the USD figure. Vendor prices change without notice; re-check before buying. Figures marked ⚠ could not be confirmed against an official source.
>
> **Study time** is my estimate, not a vendor figure. Baseline assumed: **you, today** — strong networking, working IT/security job, self-study 8–12 h/week. Someone starting cold should roughly double these. Ranges are wide because prior exposure dominates everything else.
>
> **Cost ≠ value.** The most expensive certs here (GIAC at ~$9k) are priced for employer budgets, not individuals. Several of the cheapest (BSCP at $99, CRTP at $249) are better respected per euro spent. Never self-fund a GIAC cert.

---

# [[05 Networking|Networking]]

## Certification Ladder

| Level | Certification | Focus | Cost | Study time | Valid |
|---|---|---|---|---|---|
| Associate | CCNA | Networking fundamentals | **$300** | 2–4 months | 3 yr |
| Professional | CCNP Enterprise / SP | Advanced routing, switching, wireless, automation | **$700** (core $400 + concentration $300) | 6–9 months | 3 yr |
| Expert | CCIE | Enterprise-scale networking & troubleshooting | **$2,000** ($400 written + $1,600 lab) | 12–24 months | 3 yr |
| Architect | CCDE → CCAr | Enterprise architecture & network design | CCDE ~$450 written + ~$3,700 practical ⚠ | 12–24 months | 3 yr |

> [!note] The CCIE figure understates it badly
> $2,000 is exam fees only. The lab is 8 hours, in-person, at one of a small number of global centres — add flights, hotel, and the near-certainty of a second attempt. Realistic all-in for a first-time CCIE from Europe: **$5,000–8,000 and 1–2 years**. Also worth knowing before you commit: the market has been shifting toward cloud and automation skills, and CCIE's prestige is no longer the automatic door-opener it was a decade ago.

### Alternative Vendors

| Cert | Focus | Cost | Study time |
|---|---|---|---|
| Juniper JNCIS → JNCIP → JNCIE | Juniper routing/switching | ~$300–400 per exam ⚠ · JNCIA often free promos | 2–6 months each |
| Cisco / Arista Datacenter | DC fabrics | $300–400 per exam ⚠ | 3–6 months |
| CWNA → CWNP | Wireless | ~$275 CWNA ⚠ | 2–4 months |

---

# IT Support

| Cert | Cost | Study time | Valid | Note |
|---|---|---|---|---|
| CompTIA A+ | **$548** (two exams) | 2–3 months | 3 yr | Skip — below your level |
| CompTIA Network+ | **$399** | 1–2 months | 3 yr | Redundant if you hold CCNA |
| CompTIA Security+ | **$439** (from June 2026) | 1–2 months | 3 yr | HR filter cert; required for some FR/CH public-sector roles |
| RHCSA | ~$500 ⚠ | 2–4 months | 3 yr | Genuinely hands-on, unlike the CompTIA line |
| Microsoft MS-102 | ~$165 | 1–2 months | 1 yr | Only if doing M365 admin work |

> Given your background, this whole tier is mostly a waste of money **unless** an employer or alternance contract specifically demands it. A+ and Network+ certify things you can already do.

---

# [[04 Operating Systems|Systems Administration]]

> **The domain closest to your actual day job.** Sysadmin certs are unusual in that the good ones are *performance-based* — you fix a broken system in a live environment rather than answer multiple choice. That makes them harder to fake and more respected than the CompTIA tier, and it makes them genuinely useful preparation for offensive work: you cannot attack Active Directory or escape a hypervisor properly without first knowing how an admin builds and defends one.

## Linux

| Cert | Cost | Study time | Valid | Format |
|---|---|---|---|---|
| ⭐ **RHCSA** (EX200) | **$500** | 2–4 months | 3 yr | **Hands-on lab, 3 h** |
| **RHCE** (EX294) — Ansible automation | **$500** | 3–5 months | 3 yr | Hands-on lab |
| **LFCS** (Linux Foundation) | **$445** exam · **$625** with THRIVE subscription | 2–3 months | 3 yr | Hands-on lab |
| **LPIC-1** | **$400** (2 exams × $200) | 2–4 months | 5 yr | Multiple choice |
| **LPIC-2** | **$400** (2 exams × $200) | 3–5 months | 5 yr | Multiple choice |
| RHCA | $500 per exam × 5 | 2–3 yrs | 3 yr | Hands-on |

> [!tip] Which Linux cert, honestly
> **RHCSA is the one that matters in France and Switzerland.** RHEL dominates enterprise and public-sector infrastructure there, the exam is purely practical, and recruiters recognise it. LPIC is vendor-neutral and cheaper per exam but multiple-choice, which the market reads as weaker evidence. LFCS sits in between and is the best value if you want a hands-on credential without the Red Hat price.
>
> **RHCE is worth more than it looks** — it's really an Ansible certification, which means it doubles as your entry into [[10 DevOps|DevOps]] and infrastructure-as-code. Better return than most DevOps-branded certs.

## Windows Server & Active Directory

| Cert | Cost | Study time | Valid | Note |
|---|---|---|---|---|
| **AZ-800** Administering Windows Server Hybrid Core Infrastructure | **$165** | 2–3 months | 1 yr | ⚠ **retires 30 Sept 2026** |
| **AZ-801** Configuring Windows Server Hybrid Advanced Services | **$165** | 2–3 months | 1 yr | ⚠ **retires 30 Sept 2026** |
| **AZ-802** (consolidated replacement) | **$165** ⚠ | 3–5 months | 1 yr | Beta since June 2026 |
| MS-102 (M365 admin) | **$165** | 1–2 months | 1 yr | Only if doing M365 work |

> [!danger] Deadline — about two months from now
> **AZ-800 and AZ-801 retire on 30 September 2026** and are being merged into the single **AZ-802**. If you want the two-exam path (some employers still list it by name), you'd need to sit both before then — realistically too tight from a standing start. Unless you're already most of the way through, **wait for AZ-802**: one exam instead of two, $165 instead of $330, and it won't be obsolete on arrival.

**Active Directory** is where sysadmin and red team meet directly. Your [[Credential Playbook]] and [[Technique Catalog]] notes attack exactly what AZ-800/802 teaches you to build — and **CRTP ($249)** in the offensive section above is arguably a better AD education than the Microsoft path, from the other direction. Doing both is unusually strong positioning.

---

# Virtualization & Hypervisors

> The layer your infrastructure job actually runs on, and a domain in genuine upheaval right now — which creates both risk and opportunity.

| Platform | Cert | Cost | Study time | Valid |
|---|---|---|---|---|
| **VMware** | VCP-VCF Administrator | **$250** exam | 3–5 months | no expiry (Broadcom dropped recert) |
| **VMware** | VCP-DCV (Data Center Virtualization) | **$250** | 3–5 months | — |
| **VMware** | VCAP (Advanced Professional) | **$250** | 6–9 months | — |
| **VMware** | VCIX / VCDX (Design Expert) | VCDX defence ~$4,000 ⚠ | 1–3 yrs | — |
| **Proxmox VE** | Certificate of Completion (training only) | **€1,490** single module · **€2,760** full bundle | 28 h course | — |
| **Nutanix** | NCP-MCI (Multicloud Infrastructure) | **~$199** | 2–4 months | 2 yr |
| **Hyper-V** | Covered by AZ-800/802 | **$165** | — | 1 yr |
| **Citrix** | CCA-V / CCP-V | ~$300 per exam ⚠ | 2–4 months | 3 yr |

> [!note] Broadcom changed the calculus — read this before choosing
> Since Broadcom acquired VMware, licensing was restructured toward large-enterprise subscription bundles, and a substantial number of SMEs and mid-market shops have been evaluating or executing migrations away from vSphere. Two consequences that pull in opposite directions:
>
> - **VCP is still the enterprise standard** and large FR/CH organisations, hosting providers and integrators continue to hire against it. At **$250 with no training course mandated for your first VCP** and no recertification treadmill, it is now genuinely good value — better than it was pre-acquisition.
> - **But the growth is in the alternatives.** Proxmox VE in particular has absorbed a lot of SMB migration, which is precisely the segment most FR/CH ESNs and MSPs serve. Hands-on Proxmox skill is currently in demand and *undercertified*.
>
> My read, stated as opinion rather than fact: **get the VCP for the label, learn Proxmox for the work.** If your employer runs vSphere, VCP at $250 is an easy ask. If they're mid-migration, the migration itself is the more valuable thing to be able to talk about in an interview.

> [!warning] Don't pay for Proxmox training
> Proxmox has **no real certification** — the €1,490+ training yields a *Certificate of Completion*, not a proctored credential. That is poor value for an individual. Proxmox is free, open source, and runs on hardware you already have. **Build a 3-node cluster in your [[16 Home Lab Projects|home lab]], document the migration from ESXi, and write it up.** That evidence outperforms the certificate at zero cost, and the writeup is portfolio material for exactly the SMB migration work that's currently in demand.

## Backup & storage (adjacent, and usually your responsibility anyway)

| Cert | Cost | Status |
|---|---|---|
| **Veeam VMCE** | Training ~$4,000 · exam voucher separately ⚠ | ⚠ **In transition** — VMCE/VMCA retired; VMCE+ and VMCSE announced for 2026, pricing not yet published |
| NetApp NCDA · Dell PowerStore | ~$200–300 per exam ⚠ | Only if your employer runs that hardware |

> **Don't buy a Veeam certification right now.** The old track was retired and the replacement exams weren't fully priced or released as of July 2026. Wait for VMCE+ / VMCSE to stabilise. Meanwhile, running Veeam Community Edition (free, 10 instances) in your home lab is more useful than the paper.

## What this is worth to you specifically

You already work as an **IT Infrastructure & Cybersecurity Engineer** — this section is the one place on this page where certification maps directly onto work you do today, which makes it both the easiest employer-funded ask and the fastest to study for, because you have production exposure.

**Sensible minimum path:**

| Priority | Cert | Cost | Why |
|---|---|---|---|
| 1 | **RHCSA** | $500 | Practical, respected in FR/CH, proves Linux properly |
| 2 | **VCP-VCF Administrator** | $250 | Cheap now, no recert, still the enterprise label |
| 3 | **AZ-802** (from autumn 2026) | $165 | Windows Server + AD, one exam instead of two |
| — | Proxmox + Veeam CE in the lab | **$0** | The skills without the paper |

**Total: ~$915** — roughly a fifth of your offensive ladder, for the credentials most likely to be reimbursed by your current employer since they cover systems you administer for them.

**Connects to:** [[04 Operating Systems]] · [[Linux]] · [[Windows]] · [[System Hardening]] · [[09 Cloud]] · [[16 Home Lab Projects]] · [[Cloud & Datacenters]]

---

# [[07 Programming|Programming & Software Engineering]]

> **You learn to code by *building*, not watching.** Goal: understand *others'* code **and** write your own for any need. The path = one solid curriculum → real projects → rebuild real tools → read open source. *(Pick ONE curriculum and finish it — [[Personal weaknesses|don't tutorial-hop]].)*

## Where to actually learn (expert-endorsed, project-based)

**Core curriculum — pick one, finish it:**

| Resource | Cost | Time |
|---|---|---|
| **CS50x** (Harvard) — C → Python → SQL → JS; how computers *really* work | **Free** (certificate ~$219 via edX) | 100–200 h |
| **The Odin Project** — 100% project-based full-stack | **Free** | 500–1000 h |
| **boot.dev** — backend Python/Go, gamified *(you already use it — keep going)* | **~$348/yr** or $49/mo | 300–600 h |
| **Full Stack Open** (Helsinki) — React/Node/GraphQL | **Free** (ECTS credits free too) | 150–250 h |
| **freeCodeCamp** | **Free** | 300+ h per cert |

**The reality courses skip:**

- **MIT "The Missing Semester of Your CS Education"** — shell, git, vim, debugging, tooling. **Free**, ~15 h. Essential and short.

**Think & read code like a pro (books):** ~$25–60 each

- **"The Pragmatic Programmer"** (Hunt & Thomas) — how pros actually work.
- **"Code"** (Charles Petzold) — software↔hardware from first principles.
- **"Clean Code"** (Martin) — readable code (read critically).
- **SICP** — **free online**; hard but transformative for *how to think*.
- **DSA:** **NeetCode** (free tier / ~$79 lifetime) · **"Grokking Algorithms"** · CLRS (reference). Deep CS: **Berkeley CS61A/CS61B**, **MIT 6.006** — all **free**.

**Python (your primary):** "Python Crash Course" · "Automate the Boring Stuff" (**free online**) · "Fluent Python" (advanced). For systems/memory: K&R "The C Programming Language".

## Projects that make you an expert (complete, ambitious)

| Resource | Cost | Time |
|---|---|---|
| **Codecrafters.io** ⭐ — build your own Git / Redis / HTTP server / SQLite / shell / DNS server | **~$40/mo** (free tier exists) | 20–60 h per track |
| **"Build Your Own X"** (GitHub) | **Free** | varies |
| **"Crafting Interpreters"** (Nystrom) | **Free online** (~$40 print) | 40–80 h |
| **Nand2Tetris** | **Free** | 100–150 h |
| **Advent of Code** | **Free** | ~50 h/year |

*Certs (secondary):* PCEP ~$59 · PCAP ~$295 · freeCodeCamp **free** — **projects prove more**. Do not spend money here.

**Connects to:** [[07 Programming]] · [[Data Structures & Algorithms]] · [[Software Engineering]] · [[Programming Foundations]]

---

# [[02 Electronics|Electronics & Embedded]]

> **Learn by building circuits and breaking them**, then program microcontrollers to bridge hardware↔software. This directly feeds hardware-hacking. Electronics is **portfolio-driven — few respected certs exist; build things and document them.** Budget for *components*, not exams.

## Where to actually learn (expert-endorsed, hands-on)

**Understand-by-building — the gold standard:**

| Resource | Cost | Time |
|---|---|---|
| **Ben Eater (YouTube)** ⭐ — 8-bit breadboard computer | **Free** videos · kits **$300–400** for full set | 40–80 h |
| **Nand2Tetris** — NAND gates → OS | **Free** | 100–150 h |
| "Code" (Petzold) → "But How Do It Know?" (Scott) → Nand2Tetris | ~$25–40 each | — |

**Hands-on electronics fundamentals:** books $30–120 each

- **"Make: Electronics"** (Platt) — best true-beginner hands-on book.
- **"Practical Electronics for Inventors"** (Scherz & Monk) — bench reference.
- **"The Art of Electronics"** (Horowitz & Hill, ~$100) — the professional bible; pair with **"Learning the Art of Electronics"** (Hayes).
- **All About Circuits** (free) · **Falstad** (free) · **LTspice** (free).
- YouTube: **EEVblog** · **GreatScott!** · **ElectroBOOM** — all free.

**Microcontrollers / embedded:**

- **Arduino** starter kit **$35–80** · **ESP32** boards **$5–15** each. Adafruit & SparkFun tutorials free.
- **KiCad** free; PCB fab (JLCPCB/PCBWay) **~$5–30** for 5 boards + shipping.

## Projects that make you capable

Realistic **total bench budget: $200–500** for kit, multimeter, components, dev boards — this replaces exam fees entirely in this domain.

- **Ben Eater 8-bit breadboard computer** ⭐ — you'll understand CPUs forever.
- Bench power supply · digital clock · function generator · LED cube · simple synth.
- **ESP32 + Home Assistant** sensors.
- **Design a PCB** → KiCad → fab.
- **Hardware-hacking crossover:** UART/JTAG to read a router's firmware; RFID / sub-GHz tool → [[Ports, Interfaces & Sockets]].

**Connects to:** [[02 Electronics]] · [[Digital Logic & Microcontrollers]] · [[Passive Components]] · [[Electrical Engineering & Electricity]] · [[Ports, Interfaces & Sockets]]

---

# [[06 Cybersecurity|Cybersecurity]]

> ⭐ **Your red-team-first ladder at a glance:** **CPTS** *(now)* → **CBBH / PNPT** → **OSCP** *(the anchor)* → **CRTO** *(AD)* → **OSWE / OSEP** → later CRTL / OSCE³ / GXPN.
> *FR/CH reality:* OSCP opens the doors (~90% of senior pentest offers); **PASSI/ANSSI** audit work wants OSCP + CRTO/OSWE + Bac+5; top ESNs (Synacktiv, Wavestone, Intrinsec) hire on **portfolio + report quality**. Full analysis → [[Cyber Career — Roles, Skills & Certs (Market Research)]].

## Blue Team / SOC

| Cert                         | Level        | Cost                                                         | Study time | Valid  |
| ---------------------------- | ------------ | ------------------------------------------------------------ | ---------- | ------ |
| GRC Mastery                  | Beginner     | **~$499**                                                    | 1–2 months | —      |
| ⭐ **HTB CDSA**               | Intermediate | **$490/yr** Silver (incl. voucher) · voucher alone **~$210** | 3–5 months | 3 yr   |
| BTL1                         | Intermediate | **£399** (~$540)                                             | 2–4 months | 3 yr   |
| ⭐ **GCFA** *(DFIR flagship)* | Advanced     | **~$9,000+** (SANS FOR508 ~$8,500 + exam $979)               | 3–6 months | 4 yr   |
| GCFE                         | Advanced     | ~$9,000+                                                     | 3–6 months | 4 yr   |
| CHFI · GNFA · BTL2           | Advanced     | CHFI ~$1,100 ⚠ · GNFA ~$9,000+ · BTL2 ~£1,500 ⚠              | 3–6 months | varies |

**Specializations:** Splunk (free training, ~$130 exams ⚠) · Elastic (~$500 ⚠) · Wazuh (free/open source) · AWS Security Specialty **$300** · Azure SC-200 **$165**

> [!danger] The GIAC problem
> A single GIAC exam attempt is **$979**, and the required SANS course runs **$5,000–8,500** — call it **$9,000 all-in per certification**. GCFA is a genuinely excellent DFIR credential and it is your Field-Paths top pick, but at that price it is an **employer-funded cert, not a self-funded one**. If DFIR is the goal, the sequence is: get hired somewhere with a training budget, then ask for it. Self-funding a GIAC is close to never rational — BTL1 at £399 or HTB CDSA at $490 gets you most of the way for 5% of the cost.

---

## Red Team / Offensive

**Recommended progression** (⭐ = for you, numbered in order — pair each with a project/writeup):

### Foundations — practical, affordable, lab-based

| Cert                             | Cost                                                                | Study time | Valid | Notes                                                                                 |
| -------------------------------- | ------------------------------------------------------------------- | ---------- | ----- | ------------------------------------------------------------------------------------- |
| ⭐1 **HTB CPTS**                  | **$490/yr** Silver Annual (incl. voucher) · voucher alone **~$210** | 4–8 months | 3 yr  | *Your current track.* Best value in offensive security. (IppSec youtube for guidance) |
| ⭐2 **HTB CBBH** (web/bug bounty) | Same subscription · **~$210** voucher                               | 3–5 months | 3 yr  | Now sometimes branded **CWES** ⚠                                                      |
| ⭐2-alt **TCM PNPT**              | **$499** (incl. lifetime free retake + 12 mo training)              | 2–4 months | 3 yr  | Best report-writing practice of any cert at this level                                |
| eJPT (INE)                       | **$249–299**                                                        | 1–2 months | 3 yr  | Below your level — skip                                                               |
| PJPT (TCM)                       | **$249** (incl. course + 2 attempts)                                | 1–2 months | 3 yr  | Skip if going straight to CPTS                                                        |
| eWPT (INE)                       | **$450**                                                            | 2–4 months | 3 yr  | BSCP is better value                                                                  |

### Core — the market anchor

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| ⭐3 **OSCP / OSCP+** | **$1,749** course+labs+1 attempt · **$2,749/yr** Learn One (2 attempts) · retake **~$249** | **6–12 months** | OSCP lifetime / OSCP+ 3 yr |
| **BSCP** (PortSwigger) | **$99** exam · **+$449/yr** Burp Suite Pro required | 1–3 months | 5 yr |

> [!tip] BSCP is the single best value on this entire page
> $99 for a well-respected, genuinely hard, hands-on web certification. Even counting the Burp Pro licence you'd want anyway, it costs a fraction of eWPT or OSWE and teaches the same core skill. Do this one early — probably before OSCP.

### Active Directory

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| ⭐4 **CRTO** (Zero-Point) | **£399** (~$540, incl. 40 h lab) · course-only £365 | 2–4 months | 3 yr |
| CRTP (Altered Security) | **$249** (30-day lab) · $299 bootcamp | 1–3 months | 3 yr |
| CRTE (Altered Security) | ~$549 ⚠ | 2–4 months | 3 yr |

> CRTO at £399 covering C2 + AD red teaming is the best price-to-market-value ratio in your ladder after CPTS. ~41% of senior AD roles reference it.

### Web specialism

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| ⭐5 **OSWE** (WEB-300) | **$1,749** bundle · or included in Learn One **$2,749/yr** | 4–8 months | lifetime |
| eWPTX (INE) | ~$500 ⚠ | 3–6 months | 3 yr |

### Advanced red team / evasion

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| **OSEP** (PEN-300) | **$1,749** bundle · or Learn One | 5–9 months | lifetime |
| **OSED** (EXP-301) | **$1,749** bundle · or Learn One | 6–12 months | lifetime |
| **OSCE³** (OSEP+OSWE+OSED) | **$6,099/yr** Learn Unlimited is the sane route | 18–30 months | — |
| **CRTL** (Zero-Point) | ~£549 ⚠ | 3–6 months | 3 yr |
| GIAC GPEN · GWAPT · GXPN | **~$9,000 each** all-in | 3–6 months each | 4 yr |

> [!note] OffSec bundling maths
> Individual bundles are $1,749 each. **Learn One at $2,749/yr** gives you PEN-200 plus one other 200/300-level course with two exam attempts — so if you plan OSCP **and** OSWE within twelve months, Learn One is cheaper than two bundles ($2,749 vs $3,498). **Learn Unlimited at $6,099/yr** only pays off if you're realistically clearing three or more OffSec exams in a year, which almost nobody does alongside a job. The **Aspire discount** cuts Learn One by 10/15/20% once you hold 1/2/3+ OffSec certs.

**Supporting Skills** (no exam cost — lab time only): Burp Suite · Active Directory · C2 Infrastructure · OPSEC · Windows/Linux Privilege Escalation

---

## 💰 What your ⭐ ladder actually costs

| Step | Cert | Cost | Cumulative | Time |
|---|---|---|---|---|
| ⭐1 | HTB CPTS (Silver annual) | $490 | $490 | 4–8 mo |
| ⭐2 | HTB CBBH (same sub) | ~$210 | $700 | +3–5 mo |
| — | BSCP + Burp Pro *(suggested addition)* | $548 | $1,248 | +1–3 mo |
| ⭐3 | OSCP (Learn One) | $2,749 | $3,997 | +6–12 mo |
| ⭐4 | CRTO | ~$540 | $4,537 | +2–4 mo |
| ⭐5 | OSWE (within same Learn One year) | $0 extra | $4,537 | +4–8 mo |

**Realistic total: ~$4,500–5,000 over 2–3 years**, plus 20% VAT in France (~$5,400–6,000 / ~€5,000–5,500).

Two ways that number moves:

- **Sequencing OSCP and OSWE inside one Learn One year saves ~$1,750.** That's the single biggest lever, and it requires planning the year deliberately rather than buying certs as you feel ready.
- **Employer funding.** You work in IT infrastructure and security. Most FR/CH employers have a training budget and an *alternance* context makes this easier, not harder. Ask before self-funding anything above $500 — and note that many will fund the cert in exchange for a retention clause, which is a real trade-off worth reading carefully.

---

# [[09 Cloud|Cloud]]

## AWS — flat pricing: Foundational $100 · Associate $150 · Professional/Specialty $300

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| Cloud Practitioner | **$100** | 2–4 weeks | 3 yr |
| Solutions Architect Associate | **$150** | 2–3 months | 3 yr |
| Solutions Architect Professional | **$300** | 4–6 months | 3 yr |
| Security Specialty | **$300** | 3–4 months | 3 yr |
| AI Practitioner | **$100** | 3–6 weeks | 3 yr |
| Machine Learning Engineer Associate | **$150** | 3–5 months | 3 yr |

> Passing any AWS exam gives a **50% discount voucher** for your next one, valid 12 months. Stacking these deliberately roughly halves the cost of a multi-cert path.

## Microsoft Azure — Fundamentals $99 · Associate $165 · Expert $165

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| AZ-900 | **$99** (often **free** via Microsoft Virtual Training Days) | 2–3 weeks | permanent |
| AZ-104 | **$165** | 2–3 months | 1 yr (free renewal) |
| AZ-305 | **$165** | 3–4 months | 1 yr |
| AI-900 | **$99** (free voucher events) | 1–2 weeks | permanent |
| AI-102 | **$165** | 2–3 months | 1 yr |
| SC-200 | **$165** | 2–3 months | 1 yr |

> Watch for **Microsoft Virtual Training Days** — they regularly include a free voucher for the Fundamentals exams. Paying $99 for AZ-900 or AI-900 is usually avoidable.

## Google Cloud — Foundational $99 · Associate $125 · Professional $200

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| Associate Cloud Engineer | **$125** | 2–3 months | 3 yr |
| Professional Cloud Architect | **$200** | 3–5 months | 2 yr |
| Professional Data Engineer | **$200** | 3–5 months | 2 yr |
| Professional ML Engineer | **$200** | 3–5 months | 2 yr |

## Others

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| **CARTP** (Altered Security, cloud red team) | ~$549 ⚠ | 2–4 months | 3 yr |
| **CKS** (Kubernetes Security) | **$445** (incl. 1 free retake) — **requires CKA first ($445)** | 2–4 months | 2 yr |

---

# [[10 DevOps|DevOps & Platform Engineering]]

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| AWS DevOps Engineer Professional | **$300** | 4–6 months | 3 yr |
| Azure DevOps Engineer (AZ-400) | **$165** | 3–4 months | 1 yr |
| Google Cloud DevOps Engineer | **$200** | 3–5 months | 2 yr |

**Recommended Skills** (free to learn, no exam needed): Docker · Kubernetes · Terraform · GitHub Actions · Ansible · Linux

> Terraform Associate (**$70.50**) is cheap and genuinely useful. Docker has no meaningful certification — build things instead.

---

# Data Engineering

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| Google Professional Data Engineer | **$200** | 3–5 months | 2 yr |
| Databricks Data Engineer Associate | **$200** ⚠ | 2–3 months | 2 yr |
| Snowflake SnowPro Core | **$175** ⚠ | 2–3 months | 2 yr |
| AWS Data Engineer Associate | **$150** | 3–4 months | 3 yr |
| Microsoft DP-600 | **$165** | 2–3 months | 1 yr |

---

# [[14 AI|Artificial Intelligence]]

## 1. AI Foundations

| Resource | Cost | Time |
|---|---|---|
| AI for Everyone (Andrew Ng) | **Free** to audit · ~$49 cert | 6–10 h |
| Google AI Essentials | **~$49/mo** Coursera | 10–20 h |
| Machine Learning Specialization (Ng) | **~$49/mo** Coursera | 60–90 h |
| Deep Learning Specialization (Ng) | **~$49/mo** Coursera | 100–150 h |
| AI-900 Azure AI Fundamentals | **$99** (often free voucher) | 10–20 h |
| AWS AI Practitioner | **$100** | 20–40 h |
| Introduction to Claude Cowork | **Free** ⚠ | 2–5 h |

## 2. AI Engineering

| Cert | Cost | Study time |
|---|---|---|
| AI-102 Azure AI Engineer | **$165** | 2–3 months |
| AWS ML Engineer Associate | **$150** | 3–5 months |
| Google Professional ML Engineer | **$200** | 3–5 months |
| IBM Data Science Professional | **~$49/mo** Coursera | 3–6 months |

**Topics:** Neural Networks · Transformers · Vector Embeddings · Fine-Tuning · RAG · Function Calling · AI APIs

## 3. LLM Engineering

| Resource | Cost | Time |
|---|---|---|
| OpenAI Academy | **Free** | 10–20 h |
| Anthropic Academy | **Free** | 10–20 h |
| LangChain Academy | **Free** | 10–20 h |
| Hugging Face Course | **Free** | 30–50 h |

**Topics:** Prompt Engineering · Agents · MCP · RAG · Memory · Tool Calling · Evaluation

> This whole tier is **free** and it is the most commercially current material on this page. Unusual and worth exploiting — there is no certification worth paying for in LLM engineering yet, because the field moves faster than certification bodies do.

## 4. MLOps

| Resource | Cost | Time |
|---|---|---|
| Google Professional ML Engineer | **$200** | 3–5 months |
| Databricks ML Associate | **$200** ⚠ | 2–3 months |
| Kubeflow · MLflow | **Free** (open source) | 20–40 h |
| NVIDIA DLI | **$90** per workshop ⚠ | 8 h each |

**Topics:** Model Deployment · Monitoring · CI/CD · GPU Infrastructure · Distributed Training

## 5. AI Security ★

One of the fastest-growing fields — **and the intersection of your two strongest domains.**

| Resource | Cost | Study time |
|---|---|---|
| TryHackMe AI Security path | **~$14/mo** premium | 20–40 h |
| ISACA AAIA | ~$575 member / ~$760 non-member ⚠ | 2–4 months |
| SANS SEC545 | **~$9,000** all-in | 3–6 months |
| Microsoft Security Copilot (SC-5010 etc.) | ~$99–165 ⚠ | 10–30 h |
| Google Sec-PaLM | Not a certification ⚠ | — |

**Must Know** (no cert required — this is where portfolio beats paper): Prompt Injection · Jailbreaks · Model Theft · Data Poisoning · Adversarial ML · Secure RAG · AI Red Teaming

> [!tip] The gap worth exploiting
> AI security has **almost no established certification**, high demand, and your CPTS/OSCP track plus [[14 AI]] knowledge already positions you for it. That means the entry ticket is **public work, not credentials**: write up prompt-injection findings, build an AI red-teaming tool, publish adversarial ML experiments. In a field with no accepted cert, whoever publishes becomes the reference. This is the cheapest competitive advantage available to you on this entire page — and it feeds directly into [[AI, Neuroscience, Robotics & the Future of Artificial Humans|the interdisciplinary work]] and any future venture.

## 6. AI Governance

| Cert | Cost | Study time | Valid |
|---|---|---|---|
| **IAPP AIGP** | **$799** non-member / **$649** member (+$295/yr membership) | 2–3 months | 2 yr |
| ISACA AAIA | ~$575 member / ~$760 non-member ⚠ | 2–4 months | 3 yr |
| ISO/IEC 42001 Lead Implementer/Auditor | **$1,500–3,000** (course + exam) ⚠ | 1–2 months | 3 yr |

**Frameworks** (free to read, and you should): NIST AI RMF · **EU AI Act** · Responsible AI · AI Auditing

> For an EU-based founder or consultant, the **EU AI Act** is not background reading — it's product surface. AI governance consulting is one of the few areas where a certification genuinely opens billing opportunities, because clients need someone to sign off on compliance.

---

# Governance & Leadership

| Cert | Cost | Study time | Valid | Prerequisite |
|---|---|---|---|---|
| **CISSP** | **$749** exam + **$135/yr** AMF | 3–6 months | 3 yr | **5 years experience** |
| **CISM** | **$575** member / **$760** non-member + $50 application + $45–85/yr | 3–5 months | 3 yr | 5 years experience |
| **CISA** | **$575** member / **$760** non-member + fees | 3–5 months | 3 yr | 5 years experience |
| ISO 27001 Lead Implementer | **$1,500–3,000** (course + exam) ⚠ | 1–2 months | 3 yr | — |

> ISACA membership (~$145/yr) saves ~$185 per exam — it pays for itself immediately if you sit CISA or CISM.
>
> **All of these require ~5 years of professional experience** to become fully certified. You can pass the exam earlier and hold "Associate" status, but this tier is genuinely a *later* concern. Revisit around 2029–2030.

---

# Recommended Learning Resources

## Networking

| Resource | Cost |
|---|---|
| Jeremy's IT Lab (YouTube) | **Free** — the best free CCNA course available |
| Network Warrior (book) | ~$50 |
| Packet Tracer | **Free** |
| GNS3 | **Free** |
| Containerlab | **Free** |

## Cybersecurity

| Resource | Cost |
|---|---|
| TryHackMe | **Free** tier · **~$14/mo** premium |
| Hack The Box | **Free** tier · Labs ~$20/mo · Academy $490/yr Silver |
| CyberDefenders | **Free** tier · ~$25/mo |
| LetsDefend | **Free** tier · ~$25/mo |
| OWASP · GTFOBins · PayloadsAllTheThings · DVWA | **Free** |

## AI

| Resource | Cost |
|---|---|
| DeepLearning.AI | **Free** short courses · ~$49/mo specializations |
| Fast.ai | **Free** |
| Stanford CS229 · CS231n · CS224N | **Free** (lectures + notes online) |
| Hugging Face Course · OpenAI Academy · Anthropic Academy | **Free** |

---

# Long-Term Consultant Roadmap

Networking → Cloud → DevOps → Cybersecurity → AI Engineering → AI Security → AI Governance → Enterprise Architecture

This combination provides expertise across infrastructure, cloud, cybersecurity, artificial intelligence, governance, and enterprise-scale solution design — making it well suited for consulting and security architecture roles.

**Indicative cost of the full path:** roughly **$8,000–12,000** in exam and training fees spread over 6–10 years, assuming you avoid the GIAC track and sequence OffSec purchases well. Employer funding should cover a large share of it if you ask deliberately at each job change.

> [!note] The uncomfortable counterpoint
> This page now lists roughly 90 certifications and about $60,000 of purchasable credentials. Your own stated approach — see the ⭐ note at the top — is **projects over certs**, and the FR/CH market evidence in [[Cyber Career — Roles, Skills & Certs (Market Research)]] supports it: Synacktiv, Wavestone and Intrinsec hire on portfolio and report quality. The certifications that actually move hiring decisions for you are a short list: **CPTS, OSCP, CRTO**, and eventually one AI-security or governance credential once that market matures.
>
> Everything else on this page is optional. Costing it out makes that visible — which is the main reason to have the numbers at all.

---

## Sources

Prices verified July 2026 against: [OffSec pricing](https://www.offsec.com/pricing/individual/) · [HTB Academy subscriptions](https://help.hackthebox.com/en/articles/13677074-academy-subscriptions) · [TCM Security PNPT](https://certifications.tcm-sec.com/pnpt/) · [Altered Security CRTP](https://www.alteredsecurity.com/post/certified-red-team-professional-crtp) · [PortSwigger BSCP](https://portswigger.net/web-security/certification) · [ISC2 exam pricing](https://www.isc2.org/register-for-exam/isc2-exam-pricing) · [ISACA](https://www.isaca.org/credentialing/cism) · [SANS/GIAC affiliate pricing](https://www.sans.org/cyber-security-certifications/affiliate-pricing-giac) · [Linux Foundation CKS](https://training.linuxfoundation.org/certification/certified-kubernetes-security-specialist/) · [IAPP AIGP](https://verifywise.ai/ai-governance-library/standards-and-certifications/iapp-aigp-certification) · [Google Cybersecurity Certificate](https://www.coursera.org/professional-certificates/google-cybersecurity)

**Sysadmin & virtualization:** [Red Hat EX200](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam) · [LPI exam pricing](https://www.lpi.org/exam-pricing/) · [Linux Foundation LFCS](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/) · [Microsoft AZ-800](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-800/) · [Broadcom VCP-VCF Administrator](https://www.broadcom.com/support/education/vmware/certification/vcp-vcf-administrator) · [Proxmox training](https://www.proxmox.com/en/services/training-courses/training) · [Nutanix certifications](https://www.nutanix.com/support-services/training-certification/certifications) · [Veeam certification](https://www.veeam.com/support/training/vmce-certification.html)

**Connects to:** [[Cybersecurity Skills Roadmap]] · [[Cyber Career — Roles, Skills & Certs (Market Research)]] · [[Cyber Career — Field Paths & Alternance (France)]] · [[My Career]] · [[06 Cybersecurity]] · [[05 Networking]]
