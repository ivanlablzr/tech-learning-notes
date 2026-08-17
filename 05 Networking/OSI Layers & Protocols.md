---
type: note
tags: [networking, osi, protocols, layers, reference]
---


The **OSI model** (ISO/IEC 7498-1, 1984) is the universal 7-layer framework for networking — every protocol, standard, technology and device maps to a layer, turning networking into a coherent system. The internet actually runs the leaner **TCP/IP** model, but OSI remains the teaching and troubleshooting lens.

> This is the **canonical, deep, layer-by-layer reference**. The big application protocols keep their own notes — [[DNS]], [[HTTP]], [[TLS & SSL]] — and the end-to-end "load a webpage" walkthrough lives once in [[Master Index — Technology Vault]].

> Mnemonics — bottom-up (L1→L7): **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way. Top-down (L7→L1): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

## Overview

| L | Layer | PDU | Address | Core job | Encap. adds |
|---|---|---|---|---|---|
| 7 | Application | data | — | services to apps | — |
| 6 | Presentation | data | — | format, encrypt, compress | — |
| 5 | Session | data | — | manage dialogues | — |
| 4 | Transport | segment / datagram | **port** | end-to-end delivery | TCP/UDP header |
| 3 | Network | packet | **IP** | logical addressing + routing | IP header |
| 2 | Data-Link | frame | **MAC** | local delivery, framing | header + trailer (FCS) |
| 1 | Physical | bit / symbol | — | signals on the medium | — |

**Encapsulation** runs *down* the stack when sending (each layer wraps the one above in its header → bits on the wire) and **decapsulation** *up* when receiving. **Standards bodies:** ISO/IEC (the model), **IEEE** (802.x — L1/L2), **IETF** (RFCs — L3–L7), **ITU-T** (telecom/optical), **3GPP** (cellular), **W3C** (web), Wi-Fi Alliance, Bluetooth SIG.

---

## Layer 1 — Physical
*Transmit raw **bits** as physical signals over a medium. No addressing — just "put symbols on the wire/air/fiber."*

**Wired media & standards**
- **Twisted-pair copper:** UTP/STP — **Cat5e, Cat6, Cat6a, Cat7, Cat8**; connectors **RJ-45**, RJ-11.
- **Coaxial:** RG-6, RG-59 (cable/[[Submarine Cables|broadband]]); **DOCSIS** (cable internet).
- **Fiber optic:** **single-mode (SMF, OS1/OS2)** vs **multimode (MMF, OM1–OM5)**; connectors **LC, SC, ST, MTP/MPO**; transceivers **SFP, SFP+, QSFP, QSFP28**.
- **Ethernet PHY (IEEE 802.3):** 10BASE-T, 100BASE-TX, **1000BASE-T**, 10GBASE-T/SR/LR, 25/40/100/400GbE.
- **Optical transport:** **SONET/SDH**, **OTN**, **DWDM/CWDM**, **PON / GPON / XGS-PON** (fiber-to-the-home); see [[Submarine Cables]].
- **Serial/bus:** RS-232, RS-485, USB (PHY), I²C/SPI (embedded), HDMI/DisplayPort (display).
- **Access/WAN:** DSL/ADSL/VDSL, T1/E1, ISDN (legacy).

> Full **physical connector catalogue** (USB/USB-C, RJ-45, fiber LC/SC, HDMI, audio, COM/serial, LPT/parallel…) → [[Ports, Interfaces & Sockets]] §2.

**Wireless / RF media & standards** ([[Network Media & Links]], [[Wireless & Cellular]])
- **Wi-Fi PHY:** IEEE **802.11** a/b/g/n/ac/**ax (Wi-Fi 6)**/be (Wi-Fi 7).
- **Cellular PHY:** GSM/UMTS/**LTE/5G NR** (3GPP) — OFDMA, MIMO.
- **Short-range/IoT:** **Bluetooth** (802.15.1), **Zigbee/Thread** (802.15.4 PHY), **LoRa**, **Z-Wave**, **NFC**, **RFID**
- **Satellite/space:** RF bands (L/S/C/X/Ku/Ka), see [[Space & Satellite Networks]].

**Signaling concepts**
- **Encoding / line coding:** NRZ, NRZ-I, Manchester, 4B/5B, 8B/10B, 64B/66B, **PAM-4**.
- **Modulation:** ASK, FSK, PSK, **QAM** (16/64/256/1024-QAM), **OFDM/OFDMA**, spread spectrum (**DSSS, FHSS**).
- **Properties:** bandwidth, baud vs bit rate, attenuation, noise, **SNR**, crosstalk, EMI, **simplex / half-duplex / full-duplex**, baseband vs broadband, latency.

**Devices:** cables, connectors, **transceivers (SFP/QSFP)**, **repeaters, hubs**, media converters, antennas, modems (physical), NIC PHY.
**Security:** wiretapping, jamming, **TEMPEST/emanations**, rogue APs, hardware implants → physical security, shielding, 802.1X (port control).

---

## Layer 2 — Data-Link
*Node-to-node delivery on a **single link**; framing, **MAC addressing**, media access, error detection. Split into **LLC** (IEEE 802.2 — interface to L3) and **MAC** sublayers.*

**Addressing:** 48-bit **MAC address** (EUI-48; first 24 bits = **OUI** vendor ID). EUI-64 for IPv6.

**LAN / framing standards (IEEE 802)**
- **Ethernet — IEEE 802.3** (frame format, **CSMA/CD** historically).
- **Wi-Fi MAC — IEEE 802.11** (**CSMA/CA**, RTS/CTS).
- **VLANs — IEEE 802.1Q** (tagging), **802.1ad** (QinQ stacking), **802.1p** (CoS priority).
- **Loop prevention — 802.1D STP**, **802.1w RSTP**, **802.1s MSTP**.
- **Link aggregation — 802.3ad / 802.1AX (LACP)**.
- **Access control — 802.1X** (port-based NAC), **MACsec — 802.1AE** (L2 encryption).

**Resolution & discovery**
- **ARP** (IPv4 → MAC; sits at the L2/L3 boundary), **RARP**; **NDP** for IPv6 (L3).
- **LLDP** (vendor-neutral), **CDP** (Cisco), **VTP/DTP** (Cisco trunk/VLAN).

**WAN data-link (legacy/telco):** **PPP, PPPoE**, HDLC, Frame Relay, ATM. **MPLS** is the famous **"Layer 2.5"** (labels between L2 and L3).
**IoT/PAN MACs:** 802.15.4 (Zigbee/Thread), Bluetooth **L2CAP**, **LoRaWAN** MAC.

**Concepts:** **frame**, **FCS/CRC** (error *detection*), MTU & jumbo frames, **collision domain vs broadcast domain**, unicast/multicast/broadcast.
**Devices:** **switches (L2)**, bridges, **wireless access points**, NIC (MAC).
**Security:** **ARP poisoning**, MAC flooding/spoofing, **VLAN hopping** → DHCP snooping, **Dynamic ARP Inspection**, port security, 802.1X, MACsec.

---

## Layer 3 — Network
*Logical addressing and **routing** of **packets** across multiple networks; fragmentation.*

**Addressing:** **IPv4** (32-bit, dotted decimal) & **IPv6** (128-bit, hex); **subnetting, CIDR, VLSM**; public vs private (RFC 1918); see [[Network Devices]].

**Core protocols (IETF)**
- **IPv4, IPv6** (the packet itself).
- **ICMP / ICMPv6** (errors, ping/traceroute), **IGMP** (multicast), **NDP** (IPv6 neighbor discovery, replaces ARP).
- **IPsec** — **AH** (integrity), **ESP** (encryption), **IKE/IKEv2** (key exchange) — VPNs.

**Routing protocols**
- **Interior (IGP):** **RIP** (distance-vector), **OSPF** (link-state), **IS-IS** (link-state, ISP core), **EIGRP** (Cisco hybrid).
- **Exterior (EGP):** **BGP** (the protocol that glues the internet's [[Network Infrastructure|autonomous systems]] together; runs over TCP 179).
- **First-hop redundancy:** HSRP, VRRP, GLBP.

**Address translation & overlays:** **NAT / PAT**, **GRE**, **VXLAN**, IP-in-IP, Mobile IP, SD-WAN overlays.

**Concepts:** routing table, default gateway, **TTL / hop limit**, fragmentation & path MTU, route summarization, longest-prefix match.
**Devices:** **routers**, **L3/multilayer switches**, **firewalls** (L3+).
**Security:** **IP spoofing**, **BGP hijacking / route leaks**, ICMP redirect/tunneling → **RPKI**, uRPF, ACLs, IPsec.

---

## Layer 4 — Transport
*End-to-end delivery between processes; **segmentation**, reliability, flow & congestion control. Identified by **ports**.*

**Ports & sockets:** a 16-bit **port** identifies the process; ranges are well-known **0–1023** / registered **1024–49151** / ephemeral **49152–65535**, and a **socket = IP + port + protocol**. Full ranges, the well-known-port table, **port states** & scanning → [[Ports, Interfaces & Sockets]].

**Protocols**
- **TCP** — connection-oriented: **3-way handshake** (SYN→SYN-ACK→ACK), sequence/ACK numbers, **sliding-window flow control**, **congestion control** (slow start, AIMD; algorithms **Reno, CUBIC, BBR**), ordered & reliable, graceful teardown (FIN). **MSS / windowing**.
- **UDP** — connectionless, no reliability/ordering (the app handles it). Low overhead: DNS, DHCP, VoIP, streaming, games.
- **QUIC** (IETF, over UDP) — TCP+TLS replacement powering **HTTP/3**; 0-RTT, no head-of-line blocking.
- **SCTP** (multi-streaming, telecom), **DCCP** (congestion-controlled datagrams).

**Concepts:** multiplexing/demultiplexing, reliability vs latency trade-off, segmentation & reassembly.
**Devices:** L4 load balancers, stateful firewalls (track connections).
**Security:** **SYN flood**, **port scanning** (Nmap), **UDP amplification/reflection** (DNS/NTP/memcached DDoS) → SYN cookies, rate-limiting.

---

## Layer 5 — Session
*Establish, manage, synchronize and tear down **sessions/dialogues** between applications. In TCP/IP this is mostly absorbed into the application layer.*

**Protocols/technologies**
- **RPC** (remote procedure call), **NetBIOS**, **named pipes**.
- **SMB/CIFS** session setup, **NFS** sessions.
- **SIP** (VoIP call setup/teardown), **H.323**, **RTCP** (media session control alongside RTP).
- **SOCKS** (proxy sessions), **PPTP / L2TP** (tunnel sessions), **RTSP** (streaming control).

**Concepts:** session establishment & authentication, **dialog control** (full/half-duplex), **checkpointing & recovery** (resume long transfers), token management.
**Security:** **session hijacking**, **session fixation** → secure random session IDs, timeouts, re-authentication.

---

## Layer 6 — Presentation
*Translate, **encrypt** and **compress** data into a common format — the "syntax" layer.*

**Encryption / security**
- **[[TLS & SSL|TLS / SSL]]** — encryption, integrity, authentication (handshake, certificates, forward secrecy). *Often placed at L5–6; the canonical deep note is [[TLS & SSL]].*

**Data representation**
- **Character encoding:** ASCII, **Unicode / UTF-8/16**, EBCDIC.
- **Serialization:** **XML, JSON**, ASN.1 (BER/DER), **Protocol Buffers**, YAML, MessagePack.
- **Media formats:** images **JPEG, PNG, GIF, TIFF**; audio **MP3, AAC**; video **MPEG, H.264/H.265**.
- **Compression:** **gzip / DEFLATE**, LZ77/LZMA, Brotli.
- **MIME** (content typing, also used at L7).

**Security:** SSL stripping, weak ciphers, encoding/normalization attacks → HSTS, strong cipher suites, certificate validation.

---

## Layer 7 — Application
*Protocols that deliver **services directly to applications and users**. (The big ones link to their own deep notes.)*

**Web:** **[[HTTP]] / HTTPS**, **WebSocket**, REST/GraphQL (over HTTP).
**Naming & config:** **[[DNS]]**, **DHCP / BOOTP** (UDP 67/68).
**Email:** **SMTP** (25/587), **IMAP** (143/993), **POP3** (110/995).
**File transfer:** **FTP / FTPS** (20/21), **SFTP** (over SSH), **TFTP** (UDP 69).
**Remote access:** **SSH** (22), **WinDR**, **Telnet** (23, insecure), **RDP** (3389), **VNC**.
**Management & time:** **SNMP** (161/162), **Syslog** (514), **NTP** (123).
**Directory & auth:** **LDAP / LDAPS** (389/636), **Kerberos** (88), RADIUS/TACACS+ (AAA).
**Real-time / media:** **SIP**, **RTP** (media stream), H.323.
**IoT / messaging:** **MQTT**, **CoAP**, **AMQP**, XMPP.
**File sharing:** **SMB/CIFS** (445), **NFS** (2049).

**Devices:** L7 firewalls/**WAF**, application/reverse proxies, API gateways, load balancers (L7).
**Security:** **SQLi, XSS, CSRF**, SSRF, broken auth, API abuse, phishing → input validation, **WAF**, OWASP Top 10, strong auth. Full attack-by-layer view in [[06 Cybersecurity]].

---

## Devices by layer (troubleshooting quick-ref)

| Device | Layer | Reads / acts on |
|---|---|---|
| [[Switching & Routing\|Hub]] / repeater | L1 | nothing — regenerates signal |
| [[Switching & Routing\|Switch]] | L2 | MAC addresses |
| Wireless AP | L1–L2 | radio + MAC |
| [[Switching & Routing\|Router]] | L3 | IP addresses |
| Multilayer switch | L2–L3 | MAC + IP |
| [[Switching & Routing\|Firewall]] (NGFW) | L3–L7 | IPs/ports → app payload |
| Load balancer | L4 or L7 | ports, or full HTTP |
| [[Network Devices\|NIC]] | L1–L2 | signal + MAC |

## Cross-layer & "in-between" technologies
- **ARP** — L2/L3 boundary (IP→MAC). · **MPLS** — "L2.5". · **TLS** — L5/6. · **QUIC** — blurs L4–7.
- **[[Network Types & Topologies|VPNs]]** — IPsec (L3), TLS-VPN/OpenVPN (L4–7), L2TP (L2/5).
- **[[06 Cybersecurity|Security]]** applies at *every* layer (physical locks → L1, port security → L2, ACLs/RPKI → L3, firewalls → L4–7, TLS → L6, auth/WAF → L7).
- **[[Wireless & Cellular|Cellular]]** runs its own layered stack mapping roughly onto L1–L3.

Related: [[Network Foundations]] · [[Switching & Routing]] · [[Network Devices]] · [[Network Media & Links]] · [[Network Infrastructure]] · [[DNS]] · [[HTTP]] · [[TLS & SSL]] · [[Master Index — Technology Vault]] · [[05 Networking|domain overview]]
