---
type: note
tags: [networking, topologies, wireless-lan, vpn]
---


Networks classified by **geographic scale**, the **topologies** that lay them out, the **cabling** that carries them, and the **wireless/VPN** security that protects them.

## 1. Network types by scale

From the body outward:

| Type | Scope | Tech |
|---|---|---|
| **BAN/WBAN, Nano-network** | body / molecular | IEEE 802.15.6, BLE, implantables; experimental molecular comms |
| **PAN** | personal (a few m) | Bluetooth (802.15.1), NFC, RFID, Zigbee (802.15.4), USB |
| **LAN / WLAN** | building/home | **Ethernet (802.3)**, **Wi-Fi (802.11)** |
| **VLAN** | logical LAN segment | software segmentation of one physical LAN |
| **SAN** | datacenter storage | Fibre Channel, iSCSI, FCoE — block storage to servers |
| **CAN** | campus (multi-building) | fiber backbones, VLANs |
| **MAN / WMAN** | city | Metro Ethernet, dark fiber, WiMAX (802.16) |
| **WAN / WWAN** | country/continent | leased lines, MPLS, **SD-WAN**, cellular, satellite |
| **GAN** | global | submarine cables, the Internet |

**The scale ladder** (who controls what): space/aerial (satellite **DTN**, aviation, quantum) → world-level (**[[Network Infrastructure|ASes, ISPs, IXPs, CDNs]]**; surface/deep/dark web; V2X automotive, cellular RAN) → building (cloud/datacenter, industrial OT with Modbus/OPC-UA/Profinet, campus) → individual (HAN, PAN) → molecular (BAN, nano). Specialized/ad-hoc forms like **MANET** (self-configuring mesh — Meshtastic) and blockchain networks cut across scales.

**WAN delivery** splits into **private circuits** (leased line/T-carrier, MPLS, Metro Ethernet, dark fiber, direct cloud connect — dedicated, costly) vs **public Internet circuits** (broadband DSL/cable/fiber, SD-WAN, cellular, **LPWAN** for IoT: LoRaWAN/NB-IoT/LTE-M — shared, cheap). MPLS vs SD-WAN is the current corporate rivalry.

## 2. Topologies & cabling

Physical layouts (full treatment in [[Network Foundations#4. Topologies & enterprise design|Network Foundations]]): **bus** (legacy), **ring** (Token Ring/FDDI — deterministic via token passing), **star** (modern standard), **extended/hierarchical star** (= Cisco three-tier), **mesh** (full = `N(N−1)/2` links, max redundancy — WAN cores/BGP), **point-to-point** and **point-to-multipoint** (WAN/satellite/cellular).

> **Physical ≠ logical:** a hub looks like a star but behaves as a bus (shared collision domain); a switch makes each flow point-to-point.

| Cable | Speed | Distance | Use |
|---|---|---|---|
| Cat 5e | 1 Gbps | 100 m | Gigabit Ethernet |
| Cat 6 / 6A | 10 Gbps | 55 m / 100 m | 10G Ethernet |
| Cat 8 | 40 Gbps | 30 m | datacenter top-of-rack |
| OM3/OM4 fiber (MMF) | 10–100 Gbps | 150–300 m | campus/datacenter |
| OS2 fiber (SMF) | 100 Gbps+ | 40 km+ | WAN/backbone |

## 3. Wireless LAN security

Wi-Fi security stacks **encryption + authentication + integrity**, negotiated in a **four-way handshake**:

| Gen | Cipher | Integrity | Auth |
|---|---|---|---|
| WPA | TKIP | MIC | PSK / 802.1X |
| WPA2 | **CCMP** (AES-CTR + CBC-MAC) | MIC | PSK / 802.1X |
| WPA3 | **GCMP** (AES-CTR + GMAC) | MIC | **SAE** (dragonfly), PMF, forward secrecy |

- **Personal mode** uses a pre-shared key; **Enterprise mode** uses **802.1X** with a **RADIUS** server and the supplicant→authenticator→authentication-server model (EAP methods: PEAP, EAP-TLS, EAP-FAST…). A **MIC** (message integrity check) discards tampered frames. Managed deployments use Wireless LAN Controllers (WLC) + lightweight APs over CAPWAP.

## 4. VPNs

A **VPN** encrypts traffic across a public network. Architectures: **client-to-site** (remote access — gateway + client), **site-to-site** (router/firewall gateways linking offices), **clientless/web** (SSL portal to internal apps), **peer-to-peer mesh** (e.g. Tailscale with NAT traversal). Protocols: **WireGuard**/OpenVPN (L4), **IPsec/IKEv2** (L3, site-to-site), SSL/TLS (web); legacy PPTP (broken) and L2TP (no native crypto) are dead. **ZTNA** (Zero-Trust Network Access, L7) is the modern enterprise successor — per-application access instead of full-network tunnels. Crypto detail in [[Data Encryption|VPN Encryption]].

Related: [[Network Foundations]] · [[Network Media & Links]] · [[Network Infrastructure]] · [[Wireless & Cellular]] · [[05 Networking|domain overview]]
