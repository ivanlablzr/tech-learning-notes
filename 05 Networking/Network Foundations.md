---
type: note
tags: [networking, foundations]
---


A network connects **nodes** ([[Network Devices|devices]]) over **links** ([[Network Media & Links|media]]) that follow **protocols** (rules), to move **data** reliably, efficiently, and securely from source to destination. The three core questions: **addressing** (how is each device identified? — MAC/IP, see [[Network Devices|subnetting]]), **routing** (how does data find its path?), and **reliability** (how does it arrive correctly? — TCP, checksums, retransmission).

## 1. Data — what travels the network

Everything is **binary**. Number bases used constantly in networking (hex for MAC/IP/packet bytes, group 4 bits → 1 hex digit):

| Base | Digits | Example |
|---|---|---|
| Binary (2) | 0,1 | `0b1010 = 10` |
| Decimal (10) | 0–9 | 10 |
| Hex (16) | 0–9,A–F | `0xA = 10` |

Integers are unsigned (0–255 for 8-bit) or signed via **two's complement** (see [[Digital Logic & Microcontrollers]]); reals use **IEEE-754** floats; text is encoded as bytes — **UTF-8** (variable 1–4 bytes, ASCII-compatible) dominates the web (encoding mismatches enable injection attacks).

**Measurement:** bit → byte (8 bits) → KB/MB/GB/TB. Network **speed is in bits/sec**: 1 Gbps ≈ 125 MB/s (÷8).

## 2. Signaling — bits to signals (modulation)

Data travels as **analog** (continuous) or **digital** (discrete) signals; **modulation** maps bits to a carrier:

| Technique | Idea | Used in |
|---|---|---|
| NRZ / Manchester | voltage levels / mid-bit transition | serial / 10BASE-T |
| AM / FM | vary amplitude / frequency | radio, Bluetooth |
| **QAM** | vary amplitude **and** phase | cable, Wi-Fi (64→1024-QAM) |
| **OFDM** | split across many subcarriers | Wi-Fi, LTE/5G, VDSL |

Higher-order QAM packs more bits per symbol but needs higher **SNR**.

## 3. Switching & addressing

- **Circuit switching** (PSTN): a dedicated path for the whole call — guaranteed bandwidth, wasteful when idle. **Packet switching** (the Internet): data split into independently-routed packets, reassembled by TCP — efficient and failure-resilient. All modern networks are packet-switched.
- **Cast types** (how many destinations): **unicast** (1→1, most traffic), **broadcast** (1→all on a segment — ARP/DHCP; routers block it), **multicast** (1→a subscribed group — IPTV, routing updates; built with **IGMP** host↔router and **PIM** router↔router around a Rendezvous Point), **anycast** (1→nearest of many — DNS roots, CDN edges).

## 4. Topologies & enterprise design

**Physical topologies:** bus (legacy, single point of failure), ring (FDDI/Token Ring), **star** (everything to a central switch — the norm), **mesh** (full/partial — WAN backbones, datacenters, for redundancy), tree/hierarchical.

**Enterprise design frameworks:**
- **Three-tier:** **core** (high-speed interconnect) → **distribution** (L3 routing, ACLs, inter-VLAN, HSRP/VRRP, firewalls/DMZ/IPS) → **access** (L2 switches + APs where endpoints plug in; port security + VLANs). **Two-tier** collapses core+distribution.
- **Spine-leaf** (modern datacenters — predictable east-west latency), **hub-and-spoke** (WAN/VPN), full/partial **mesh**, **flat** (small).

**Accessibility scopes:** intranet (private internal), extranet (shared with partners), Internet (public), darknet (anonymity overlay).

## 5. Protecting data

- **In transit:** TLS (HTTPS/mail), IPsec (VPN/WAN), SSH (remote shell/SFTP), WPA3 (Wi-Fi).
- **At rest:** full-disk encryption (BitLocker/LUKS/FileVault), database/file encryption.
- **Integrity:** CRC (accidental corruption), cryptographic hashes (SHA-256), digital signatures + **HMAC** (authenticity). Full treatment in [[Cryptography]] / [[Data Encryption]].
- **Availability:** [[Network Performance & Resilience|RAID]], replication, and the **3-2-1 backup rule** (3 copies, 2 media, 1 offsite).

## 6. Protocols by OSI layer

| Layer | Key protocols |
|---|---|
| 7 Application | HTTP/S, DNS, DHCP, FTP, SMTP/IMAP, SSH, SNMP |
| 6 Presentation | TLS/SSL, MIME, compression |
| 5 Session | NetBIOS, RPC, NFS, SIP |
| 4 Transport | TCP, UDP, QUIC, SCTP |
| 3 Network | IPv4/IPv6, ICMP, OSPF, BGP, IPsec |
| 2 Data-Link | Ethernet (802.3), Wi-Fi (802.11), ARP, STP |
| 1 Physical | electrical/optical/radio signals |

Full layer-by-layer detail in [[OSI Layers & Protocols]].

> **Packet analysis:** read the wire with **Wireshark** (GUI, display filters like `tcp.port==443`, `dns`, `ip.addr==…`) or **tcpdump**/**tshark** (CLI capture to `.pcap`).

Related: [[OSI Layers & Protocols]] · [[Network Devices]] · [[Network Media & Links]] · [[Network Performance & Resilience]] · [[05 Net