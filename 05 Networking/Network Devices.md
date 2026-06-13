---
type: note
tags: [networking, devices, ip-addressing]
---


The nodes on a network, the **NIC** that connects them, and the **MAC/IP addressing** that identifies them.

## 1. Node types

- **Endpoint devices** (hosts) — each has a [[|NIC]] with a unique MAC + IP: computers, phones (IP phones via SIP/PBX), printers, IoT, and **servers** (web/DB/mail/proxy/VPN; tower vs rack vs blade form factors). *Trust note:* "trusted" devices don't beacon out on their own; "untrusted" (IoT, smartphones, public-facing servers) constantly talk to outside services — segment them.
- **Intermediary devices** (move/control traffic) — repeaters/hubs, **switches**, **routers** (incl. wireless), modems, firewalls, load balancers, proxies, IDS/IPS, VPN gateways (see [[Switching & Routing]]).
- **Virtual devices** — **containers** (Docker/Podman/LXC: `Hardware → OS/kernel → container engine → app`, sharing the kernel) and **VMs** via hypervisors (**Type 1/bare-metal**: ESXi, Hyper-V, Proxmox, Xen; **Type 2/hosted**: VirtualBox, VMware Workstation). Networking detail in [[10 DevOps]] / [[09 Cloud]].

> A node is a full stack: [[03 Computer Hardware|hardware]] → firmware → kernel → [[04 Operating Systems|OS]] → middleware → application → network → user.

## 2. MAC addressing (Layer 2)

A **MAC address** is 6 bytes: the first 3 octets are the **OUI** (manufacturer ID), the last 3 identify the specific interface. **UAA** = burned-in by the vendor; **LAA** = overridden by software. Broadcast = `FF:FF:FF:FF:FF:FF`. Used on Ethernet/Wi-Fi/Bluetooth to address devices on the **same segment**; **MAC randomization** mitigates device tracking.

## 3. NIC — the bridge between logic and physics

The NIC converts between digital data and physical signals (electricity/light/radio). Path: `wire ↔ transceiver ↔ NIC chip ↔ PCIe ↔ RAM ↔ OS/CPU ↔ application`. It carries the node's two identities: **logical** (L3 IP — the routable "mailing address") and **physical** (L2 MAC).

**Transmit (egress):** app data → OS **encapsulates** (TCP/UDP + IP headers → packet) → buffered in a RAM **descriptor ring** → driver "rings the doorbell" (MMIO) → NIC **DMA**-fetches over PCIe → MAC logic **frames** it (adds MAC + CRC) → PCS encoding + FEC → **SERDES** serializes → PHY **modulates** (NRZ ≤25G, PAM4 high-speed) onto the link.
**Receive (ingress):** PHY detects + demodulates → PCS decode + FEC repair → MAC checks destination (drop if not mine) + CRC (drop if corrupt) → IRQ + DMA into RAM → OS **decapsulates** headers → delivers payload to the app.

## 4. IP addressing & subnetting (Layer 3)

An IP address identifies a NIC; it splits into a **network portion (N)** + **host portion (H)**, with the **subnet mask** (CIDR `/n` = number of N bits) marking the boundary. Subnetting borrows host bits to create subnets; **usable hosts = 2^(host bits) − 2** (network + broadcast reserved). Assigned **statically** or via **DHCP** (server hands out IP, mask, gateway, DNS from a pool; relays forward requests across subnets).

**IPv4** (32-bit, ~4.3 billion):

| Range | Purpose |
|---|---|
| `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | Private (RFC 1918) — LAN/VLAN |
| `127.0.0.0/8` | Loopback (localhost) |
| `169.254.0.0/16` | Link-local / **APIPA** (DHCP failed) |
| `100.64.0.0/10` | Carrier-grade NAT |

Legacy classes A/B/C (now superseded by **CIDR**). Public addresses flow **IANA → 5 RIRs → ISPs → customers**. IPv4 is **exhausted** (IANA pool depleted 2011; AFRINIC last in 2020) and now trades on a secondary market — driving NAT and the IPv6 transition.

**IPv6** (128-bit, 3.4×10³⁸ — effectively unlimited): hex hextets `2001:0db8:…`, compressed with `::` for all-zero groups; features **SLAAC** (stateless autoconfig), **EUI-64**, and **NDP** (replaces ARP). Adoption ~45% of Google traffic. **NAT** translates private↔public IPv4 at the network edge (and conserves addresses).

Related: [[Network Foundations]] · [[Switching & Routing]] · [[OSI Layers & Protocols]] · [[Network Infrastructure]] · [[05 Networking|domain overview]]
