---
type: note
tags: [networking, certifications, career]
---


A map of industry certifications by career track — the networking ladder in depth, plus pointers for the other domains.

## The Cisco networking ladder

| Tier | Cert | Focus |
|---|---|---|
| **Associate** | **CCNA** | fundamentals: TCP/IP, Ethernet switching, VLANs/STP, IPv4 subnetting, static/OSPF routing, IPv6, WLANs, ACLs, NAT, QoS, device security, intro automation |
| **Professional** | **CCNP** (ENCOR + concentration) | advanced L2 (RSTP/MST, EtherChannel), routing (OSPF LSA/area types, EIGRP DUAL, **BGP** iBGP/eBGP + path attributes, redistribution), HA (HSRP/VRRP/GLBP), MPLS/SD-WAN, enterprise wireless (CAPWAP), automation (NETCONF/RESTCONF/YANG) |
| **Expert** | **CCIE** | deep protocol behavior, large-scale R&S, QoS, multicast, advanced troubleshooting + automation |
| **Design/Architect** | **CCDE / CCDE→CCAr** | multi-domain architecture, failure-domain & risk analysis, business-aligned design |

> Study the way pros do: a course/book (Jeremy's IT Lab for CCNA, *Network Warrior*) → hands-on labs (**Packet Tracer / GNS3 / containerlab**) → practice exams. Theory without configuration doesn't stick

## Certifications by track

- **Networking:** CCNA → CCNP → CCIE (vendor-neutral alt: CompTIA Network+).
- **Help desk / IT support:** CompTIA A+, Network+, plus an OS admin cert (RHCSA, MS-102).
- **Blue team / SOC:** Google Cybersecurity (beginner) → GRC fundamentals → **Security+ / CySA+** → SOC analyst paths (TryHackMe SAL1, HTB **CDSA**, BTL1) with Splunk/Elastic/Wazuh → cloud security (AWS Security Specialty, Azure SC-200) → forensics (**GCFA/GCFE**, CHFI).
- **Red team / pentest:** eJPT/PT1 → **OSCP** (the HR filter — 24h practical, AD/pivoting/privesc/reporting) → HTB **CPTS**, Burp Suite web labs → **CRTO** (Cobalt Strike, C2, AD chains, OPSEC — what separates pentesters from red teamers) → **OSEP** (EDR evasion, advanced emulation).
- **Cloud:** AWS (Cloud Practitioner → Solutions Architect → Security Specialty) or Azure (AZ-104 Administrator → Solutions Architect → SC-200).
- **DevOps:** AWS/Azure/GCP DevOps Engineer.
- **Data / AI:** Google Data Engineer, IBM Data Science; Azure AI Engineer; **AI auditing** (ISACA AAIA — emerging, high-demand).
- **Governance / management:** **CISSP** (governance), CISM (management), CISA (audit), ISO 27001.


> **My Choice**: GRC Mastery -> HTB CPTS/OSCP -> Dante/Zephyr HTB -> HTB CDSA/CySA+/Security+ -> AI certs/courses (AWS AI, Azure AI, Anthropic AI, ISACA AAIA, AI security) -> Cloud (AWS/Azure) -> GHIC -> CRTO -> OSEP -> SANS Purple Team Operations Certificate -> GFCA and GFCA -> CISSP


## Practice & resources

Hands-on platforms: **TryHackMe**, **Hack The Box**, Offensive Security, CyberDefenders, LetsDefend. References: **OWASP Top 10**, **GTFOBins** (privesc), PayloadsAllTheThings, **DVWA** (vulnerable web app), and the cloudcommunity *Free-Certifications* list.

> This is the **full reference** of certs by track. For *your* ordered path — which certs, in what sequence, toward the consultant goal — see the **[[Master Roadmap — Secure AI, Cloud & Offensive Expert → Consultant → Founder|Master Roadmap]]** "path at a glance" table and work it daily from the **[[Roadmap Kanban]]**.

Related: [[Master Roadmap — Secure AI, Cloud & Offensive Expert → Consultant → Founder|Master Roadmap]] · [[Roadmap Kanban]] · [[Switching & Routing]] · [[06 Cybersecurity|Cybersecurity]] · [[09 Cloud]] · [[05 Networking|domain overview]]
