---
type: note
tags: [networking, cellular, wan, sdwan]
---


Mobile/cellular wide-area networking, the legacy phone network it grew from, and **SD-WAN** — the software-defined way modern enterprises stitch all these transports together.

## 1. Cellular evolution

| Gen | Switching | Key tech | Speed |
|---|---|---|---|
| **2G** (GSM) | circuit | TDMA, Mobile Switching Center | ~kbps |
| **3G** (UMTS) | packet | W-CDMA, PDP data tunnels, RNC | ~Mbps |
| **4G** (LTE) | packet (all-IP) | OFDM, eNodeB, EPC core | ~100 Mbps |
| **5G** (NR) | packet, service-based | OFDM, gNodeB, 5GC, network slicing | ~1 Gbps |

**Architecture (4G/5G):** the device (UE) talks to a base station (**eNodeB**/**gNodeB**) over the radio access network (RAN), which connects to a packet core. The attach flow: **RACH** (random-access "hello") → **registration/authentication** (the device's identity **IMSI** is checked against the subscriber DB; 5G conceals it as **SUCI**) → **session establishment** (a data tunnel/bearer is built — 5G's SMF tells the UPF to create a **GTP-U** tunnel) → data transmission. Modern RAN is disaggregated (**O-RAN**, small/macro cells, Cloud-RAN).

**Cellular security risks:** **IMSI catchers / Stingrays** (fake base stations harvest identities — mitigated by SUCI), **signaling-storm DoS** (IoT botnets exhaust the RRC state machine — rate-limit), **unencrypted GTP-U backhaul** (tunnel injection/teardown — enforce **IPsec** on the N3 interface), legacy **SS7/SIGTRAN** abuse, and the 5G core's REST/JSON service-based architecture (secure with **mTLS** between functions, Zero Trust). The **PSTN** is the legacy circuit-switched voice network (now largely VoIP-bridged) cellular evolved alongside.

## 2. SD-WAN — software-defined WAN

SD-WAN applies [[Switching & Routing|SDN]] to the WAN: decouple the **control plane** (policy/routing decisions in a central controller) from the **data plane** (forwarding at branch CPE). It replaces rigid, expensive **MPLS** WANs where all branch traffic was backhauled through HQ before reaching the internet/cloud.

- **Underlay vs overlay:** the **underlay** is whatever physical transport exists (MPLS, broadband, 4G/5G, satellite); SD-WAN builds an encrypted **overlay** (IPsec/TLS tunnels) on top, forming a virtual mesh independent of the underlay.
- **Transport-agnostic:** use all links *simultaneously*, continuously measuring latency/jitter/loss per tunnel. An org can drop pricey MPLS for two broadband circuits + 4G backup at a fraction of the cost.
- **Application-aware routing:** identify apps via **DPI**/DSCP, map each to an **SLA policy** (e.g. "VoIP needs <50 ms, <1% loss"), and **auto-switch** paths in milliseconds when one degrades — plus **local breakout** (send SaaS like M365 straight to the internet instead of backhauling).
- **Centralized orchestration + ZTP:** define policy once in the controller and push to every site; a new branch self-configures by authenticating to the controller (no on-site engineer).
- **Security:** IPsec by default, integrated NGFW/secure web gateway, converging into **SASE** (SD-WAN + cloud security: ZTNA/SWG/CASB as one service). Vendors: Cisco (Viptela), VMware VeloCloud, Fortinet, Palo Alto Prisma, Aruba EdgeConnect.

**Trade-offs:** cloud-hosted management depends on internet reachability (data plane keeps forwarding on last-known policy), proprietary vendor lock-in, and broadband variability it can route around but not magically fix.

Related: [[Network Types & Topologies]] · [[Switching & Routing]] · [[Network Infrastructure]] · [[09 Cloud]] · [[05 Networking|domain overview]]
