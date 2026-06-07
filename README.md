# 🧠 Tech Learning Notes

> Open study notes on how modern technology actually works — from electrons to the global tech economy — built alongside my Bachelor's degree and a self-made roadmap.

There is no "new" theory here, all the information documented can be found publicly if someone dives deep enough. The value of this repository lies in its **synthesis**. It's a new way of organizing and seeing technical domains that are usually scattered across dozens of separate resources. A guided, navigable knowledge graph ties it together.

When learning technology, it's never wise to rely on a single biased resource. This vault was forged through a blend of **academic study** (Bachelor's curriculum), **industry certifications**, **extensive online research**, and **real-world intuition** developed on the job as a Network Technician.

To truly master these domains, keep two guiding principles in mind:

- **Cross-reference** — don't trust a single source; compare explanations and solutions across multiple technical frameworks.
- **Practice over theory** — theory is a map, but implementation is the terrain. You must build, break, and lab these concepts to see how they actually behave.

> ⚠️ **These are personal learning notes, not a textbook.** I'm a student learning in public — notes may contain mistakes or out-of-date figures (especially the 2025–26 economics numbers). Always verify against primary sources. Content is licensed **CC BY 4.0** (see `LICENSE`).

---

## 🗺️ Vault Domain Map

The knowledge base is built linearly along a **dependency graph** (Foundations → Systems → AI/Quantum → Macro).

```
[Math & Physics] → [Electronics & Hardware] → [OS & Networks] → [Software & Cloud] → [AI & Quantum] → [Economy]
```

| Folder | Domain | Core focus areas |
|---|---|---|
| **00–01** | **Foundations** | Mathematics, classical physics, electromagnetic theory |
| **02–03** | **Hardware Systems** | Analog/digital electronics, microcontrollers, CPU architecture |
| **04–06** | **Infrastructure & Security** | Linux/Windows, OSI layers, cryptology, penetration testing |
| **07–13** | **Software & Operations** | Applied programming, databases, cloud, DevOps/SRE, distributed systems, architecture |
| **14–15** | **Advanced Frontiers** | Artificial intelligence (ML/LLMs), quantum computing, PQC |
| **17** | **Macro** | Tech economy, value chain, geopolitics & sovereignty |

---

## 📚 Deep-Dive Curriculum

Every topic in the knowledge base, by domain. Click a domain to open its folder; each contains an overview note plus the deep-dive notes listed below.

### 🔢 [00 — Mathematics](<00 Mathematics>)
Binary & boolean logic, discrete math (sets, graphs, combinatorics), linear algebra, probability & statistics, calculus, number theory, theory of computation.

### ⚛️ [01 — Physics](<01 Physics>)
Mechanics & forces, electromagnetism, optics (fiber/sensors), materials, simulation & modelling.

### ⚡ [02 — Electronics & Digital Systems](<02 Electronics>)
- **Electrical Engineering & Electricity** — Ohm's law, KVL/KCL, AC/DC, power, machines
- **Passive Components** — resistors, capacitors, inductors, transformers
- **Active Components & ICs** — diodes, transistors/MOSFETs, integrated circuits, fab scale
- **Digital Logic & Microcontrollers** — logic gates, Boolean algebra, CMOS, FPGA, 8/16/32-bit MCUs, GPIO/UART/SPI/I²C
- **Electromechanical Components** — motors, relays, sensors, actuators

### 🖥️ [03 — Computer Hardware](<03 Computer Hardware>)
- **CPU & Processing Units** — CPU/GPU/DPU/NPU, cache, pipelines
- **Memory** — RAM types (DRAM/SDRAM/DDR/LPDDR), latches & flip-flops
- **Storage** — HDD/SSD/NVMe, RAID
- **Motherboard & I/O** — chipset, PSU, cooling, PCIe, buses
- **Hardware Logic & Fabrication** — computational logic, photolithography

### 🐧 [04 — Operating Systems](<04 Operating Systems>)
- **Linux** — filesystem, permissions, processes, services, package management, hardening
- **Windows** — registry, services, WMI, filesystems, hardening, security tools

### 🌐 [05 — Networking](<05 Networking>)
- **Network Foundations** — what a network is, data & signaling, cast types
- **OSI Layers & Protocols** — the 7 layers, TCP/UDP, encapsulation, DNS, HTTP
- **Network Devices** — NICs, MAC, IP addressing & subnetting, DHCP
- **Switching & Routing** — VLANs/STP, routers, routing protocols, NAT, SDN, firewalls
- **Network Media & Links** — copper/fiber, RF, Wi-Fi, IoT wireless
- **Network Types & Topologies** — LAN→WAN by scale, WLAN security, VPNs
- **Internet & Infrastructure** — ISPs, ASes, BGP, IXPs, IANA/RIRs
- **Wireless & Cellular** — 2G→5G, SD-WAN
- **Space & Satellite Networks** — orbits, satellite comms, DTN, constellations
- **Network Performance & Resilience** — bandwidth/latency/jitter, redundancy, DR, RAID
- **Submarine Cables** — the physical internet, chokepoints, cut risks
- **State-Owned & Government-Linked Carriers** — who owns the carriers, and why it matters
- **[Regional Infrastructure](<05 Networking/Regional Infrastructure>)** — 14 per-region deep-dives (N. America, Latin America, W./N./S./E. Europe, Russia-CIS, Middle East, N. Africa, Sub-Saharan Africa, S./SE/E. Asia, Oceania)
- **IT Certifications** — the Cisco ladder + cross-domain cert map

### 🔒 [06 — Cyber Security](<06 Cybersecurity>)
- **Cybersecurity Foundations** — CIA triad, defensive principles, domains, careers
- **Cryptography** — symmetric/asymmetric/hashing concepts, key management, cryptanalysis
- **Data Encryption** — AES/RSA/ECC, signatures/PKI, data at rest & in transit, TLS
- **Information Security & Access** — IAM, authentication/MFA, integrity, non-repudiation, privacy
- **Network Security** — firewalls, segmentation/DMZ, IDS/IPS, secure design, Zero Trust/SASE
- **Internet & Application Security** — OWASP Top 10, web/email/DNS security, IoT & AI/ML security
- **System Hardening** — application/OS/account/server hardening, secure coding
- **Endpoint Security** — AV → EDR/XDR, device control, hardware root of trust (TPM/Secure Boot)
- **Threats & Malware** — malware taxonomy, network attack classes, APTs & MITRE ATT&CK
- **Security Operations & IR** — SOC/SIEM, incident response, forensics, threat intelligence
- **Ethical Hacking** — offensive methodology, technique catalog, AD credential playbook
- **OpSec & Physical** — physical security, social engineering, operational security

### 💻 [07 — Programming](<07 Programming>)
- **Programming Foundations** — effective code (NASA principles), architecture, P2P
- **Languages & Applied Programming** — Python, web (HTML/CSS), data analytics, microcontroller programming

### 🗄️ [08 — Databases](<08 Databases>)
Relational vs NoSQL, data modeling, indexing, transactions, query basics.

### ☁️ [09 — Cloud](<09 Cloud>)
- **Cloud & Datacenters** — IaaS/PaaS/SaaS, virtualization, regions/AZs, datacenter design

### 🔧 [10 — DevOps](<10 DevOps>)
CI/CD, Infrastructure as Code, containers, configuration management, automation.

### 📈 [11 — SRE](<11 SRE>)
Reliability, SLI/SLO/SLA, observability (metrics/logs/traces), incidents & error budgets.

### 🕸️ [12 — Distributed Systems](<12 Distributed Systems>)
Consistency models, consensus, partitioning/replication, the network as an unreliable channel.

### 🏛️ [13 — Architecture](<13 Architecture>)
System design, service boundaries, scalability & trade-offs, Zero-Trust architecture.

### 🤖 [14 — Artificial Intelligence](<14 AI>)
- **AI Foundations** — narrow/general/super AI, applications across domains
- **Machine Learning & Deep Learning** — ML, neural networks, DL, NLP, computer vision
- **LLMs & Prompting** — how LLMs work, ChatGPT, prompt engineering

### ⚛️ [15 — Quantum Computing](<15 Quantum Computing>)
Qubits & physical implementations, gates/circuits, error correction, key algorithms (Shor/Grover), the NISQ era, quantum networking, and the **post-quantum cryptography (PQC)** threat.

### 💰 [17 — Tech Economy & Geopolitics](<17 Tech Economy & Geopolitics>)
- **The Technology Value Chain** — where value is captured; the "smile curve"; chokepoints
- **Semiconductor Economics** — fabless/foundry/IDM, capital intensity, TSMC/ASML/Nvidia
- **The Platform & Digital Economy** — network effects, Big Tech business models, antitrust
- **Cloud, Data & AI Economics** — cloud oligopoly, the AI capex super-cycle, the bubble debate
- **US-China Tech War & Export Controls** — the chip war, export controls, China's countermoves
- **Tech Sovereignty & Governance** — CHIPS Acts, data sovereignty, standards, the three-bloc world

---

## 🛠️ How to Use This Vault

These notes are written in **Obsidian** Markdown, so internal links use `[[wikilink]]` syntax. On GitHub those render as plain text (still readable); for the full linked **knowledge graph**, use Obsidian:

1. **Clone the repo** to your machine.
2. **Open the folder as a vault** in Obsidian to activate the graph view and clickable links.
3. **Start** from the **Deep-Dive Curriculum** index above, or open any domain's overview note (e.g. `05 Networking.md`) and follow the links from there.

> 💡 Prefer browsing online? This vault can also be published as a website with [Quartz](https://quartz.jzhao.xyz/) or Obsidian Publish.

---

*Disclaimer: these notes are my personal technical journal and active field guide. They evolve as technology — and my hands-on experience — does.*
