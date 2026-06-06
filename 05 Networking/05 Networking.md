---
type: domain
tags: [domain, moc, networking]
---

How independent machines communicate — from electromagnetic fields with antennas or electrons on a wire to the global Internet of autonomous systems. The job: move data **reliably, efficiently, and securely** by solving addressing, routing, reliability, and naming across heterogeneous links. (Detailed map: [[Foundations (Source Docs)#3 - Networking Knowledge Map|Networking Knowledge Map]].)

## Domain notes

| Note | Covers |
|---|---|
| [[Network Foundations]] | What a network is, data/signaling, switching, cast types, topologies, protocols by layer |
| [[OSI Layers & Protocols]] | The 7 layers, TCP/UDP, encapsulation, **DNS**, **HTTP** |
| [[Network Devices]] | Nodes, NIC, MAC, **IP addressing & subnetting** |
| [[Switching & Routing]] | Switches/VLANs/STP, routers, routing protocols, NAT, **SDN** |
| [[Network Media & Links]] | Copper/fiber cabling, RF, Wi-Fi, IoT wireless |
| [[Network Types & Topologies]] | LAN→WAN by scale, cabling, WLAN security, VPNs |
| [[Internet & Infrastructure]] | ISPs, **ASes, BGP, IXPs, IANA/RIRs**; links to cables, carriers & 14 regional notes |
| [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] | the physical cables + who owns the carriers; plus 14 per-region deep-dives in [[Regional Infrastructure/North-America Digital Infrastructure\|Regional Infrastructure]] |
| [[Wireless & Cellular]] | 2G–5G, SD-WAN |
| [[Space & Satellite Networks]] | Orbits, satellite comms, DTN, constellations |
| [[Network Performance & Resilience]] | Bandwidth/latency/jitter, resilience (STP/FHRP/BGP), DR, RAID |
| [[IT Certifications]] | The Cisco ladder + cross-domain cert map |

## The five views

**Dependencies:** builds on [[01 Physics]] (the physical layer) and [[04 Operating Systems]] (sockets/stack); depended on by [[06 Cybersecurity]] (network security, TLS), [[09 Cloud]] (VPCs, load balancing), and [[12 Distributed Systems]] (the network *is* the unreliable channel).
```
Physics + OS → Networking → (Security, Cloud, Distributed Systems)
```

**Learning path:** OSI/TCP-IP models → physical & data-link (Ethernet, switching, VLANs) → network layer (IP, subnetting, ARP, routing) → transport (TCP/UDP, ports) → application (HTTP, DNS, DHCP) → internet architecture (autonomous systems, peering/transit, IXPs) → **BGP** → DNS deep dive → CDN/wireless/datacenter fabrics → security, monitoring, automation.

**Projects:** home networking lab (VLANs, routing, firewall) → DNS infrastructure (authoritative + recursive + DNSSEC) → **BGP/ISP simulation** (multi-AS in FRR, simulate a route leak) → dissect a TLS handshake in Wireshark. See [[Learning Paths & Projects#Project Roadmap]].

## Bridge topics it owns

- **TCP/IP & sockets** (OS ↔ Networking ↔ Programming) — the uniform API for cross-machine communication.
- **DNS** (Networking ↔ Applications ↔ Governance ↔ Security) — names → addresses; resolution chain; DNSSEC.
- **BGP** (ISP Infrastructure ↔ Internet) — reachability exchange between autonomous systems; multi-parent (IP + AS + TCP).

Also links to **TLS** ([[06 Cybersecurity]]) and **load balancing** ([[09 Cloud]]). Full map in [[Knowledge Graph#Bridge Topic Map]].

Related: [[01 Physics]] · [[04 Operating Systems]] · [[06 Cybersecurity]] · [[09 Cloud]] · [[Home]]
