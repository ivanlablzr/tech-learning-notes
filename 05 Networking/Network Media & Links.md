---
type: note
tags: [networking, media, cabling, wireless, rf]
---


The **physical layer** (L1) — how bits actually travel between nodes, over copper, fiber, or radio. Choose media by speed, distance, reliability, interference, security, and cost.

**Transmission direction (duplexing):** **simplex** (one-way only — broadcast radio), **half-duplex** (both ways but not at once — walkie-talkies, Wi-Fi, hubs; collisions possible), **full-duplex** (both ways simultaneously via separate Tx/Rx paths — modern switches, fiber). Pluggable transceiver modules (SFP/SFP+/QSFP) let one device port accept copper or fiber.

## 1. Copper — Ethernet cabling

Twisted-pair carries data as voltage changes (modulated NRZ/Manchester). **UTP** (unshielded) is standard for LANs; **STP** (shielded) for high-EMI environments. Pinouts: **T568A/T568B** wiring standards.

| Cat | Speed | Bandwidth |
|---|---|---|
| Cat5e | 1 Gbps | 100 MHz |
| Cat6 | 1–10 Gbps (10G ≤55 m) | 250 MHz |
| Cat6a | 10 Gbps | 500 MHz |
| Cat8 | 40 Gbps | 2 GHz |

**Straight-through** cables connect *different* device types (PC↔switch); **crossover** connect *similar* ones (switch↔switch) — but **Auto-MDI/MDIX** on modern gear auto-detects, making crossovers rare. Devices transmit on pins 1,2 (NICs/routers/APs) vs 3,6 (hubs/switches).

## 2. Fiber optics

Light travels through a glass **core** kept in by total internal reflection off the **cladding** (then buffer/strengthener/jacket). Immune to EMI, long-distance, high-speed. **Single-mode (SMF)** uses a laser for long range (10–40 km+); **multi-mode (MMF)** uses LED/laser for shorter campus/datacenter runs.

| Fiber | Core | Reach (10G) |
|---|---|---|
| OM3 (MMF) | 50 µm | 300 m |
| OM4 (MMF) | 50 µm | 550 m (100G shorter) |
| OS2 (SMF) | 9 µm | 40 km+ |

**Media comparison:**

| | UTP | MMF | SMF |
|---|---|---|---|
| Cost | low | medium | medium-high |
| Max distance | 100 m | ~550 m | 40 km+ |
| EMI / eavesdrop risk | some | none | none |

> Short-range board protocols (UART/SPI/I²C, USB) are covered in [[Digital Logic & Microcontrollers]].

## 3. Wireless — RF fundamentals

Radio = electromagnetic waves (electric + magnetic fields) carrying data via **modulation** (AM/FM/PM, digital ASK/FSK/PSK/QAM — see [[Network Foundations#2. Signaling — bits to signals (modulation)|Foundations]]). Wireless contends with collisions, interference, hidden nodes, and the hidden-terminal problem. **Antenna configurations (MIMO)** multiply throughput:

- **SISO** — one antenna each (legacy).
- **SIMO / MISO** — receive / transmit **diversity** (combine copies of one stream for reliability).
- **MIMO** — multiple antennas both ends doing **spatial multiplexing**: split data into different chunks sent simultaneously on the same channel → a 4×4 MIMO ~quadruples throughput without extra spectrum. Used in Wi-Fi 5/6/7 and 5G.

## 4. Wi-Fi (802.11)

| Gen | Standard | Max speed | Notable |
|---|---|---|---|
| Wi-Fi 5 | 802.11ac | ~3.5 Gbps | 4×4 MU-MIMO, 5 GHz |
| Wi-Fi 6 | 802.11ax | 9.6 Gbps | OFDMA, 8×8 MU-MIMO |
| Wi-Fi 7 | 802.11be | 46 Gbps | 320 MHz channels, **4K-QAM**, **MLO** (multi-link), multi-RU puncturing |

Bands: **2.4 GHz** (long range, crowded), **5 GHz** (faster, more channels), **6 GHz** (Wi-Fi 6E/7 — wide 320 MHz channels, less interference, shorter range). Identifiers: **SSID** (network name), **BSSID** (AP MAC), passphrase. Security (WPA2/WPA3, 802.1X) is in [[Network Types & Topologies#3. Wireless LAN security|Network Types]].

## 5. Short-range & IoT wireless

| Tech | Standard | Range | Rate | Mesh | Use |
|---|---|---|---|---|---|
| **Bluetooth / BLE** | 802.15.1 | ~10 m | 1–2 Mbps | BT Mesh | audio (Classic), IoT/wearables (BLE, GATT) |
| **Zigbee** | 802.15.4 | 10–100 m | 250 kbps | yes (65k nodes) | smart home (Hue, SmartThings); AES-128 |
| **Z-Wave** | sub-GHz | medium | 100 kbps | yes (~232) | home automation, less interference |
| **Thread/Matter** | 802.15.4 | — | — | yes | the new cross-ecosystem smart-home standard |
| **LoRa/LoRaWAN** | CSS, ISM | 2–15 km | 0.3–50 kbps | star-of-stars | long-range low-power IoT sensors |

**LPWAN** (long-range IoT): **LoRaWAN** (unlicensed, deploy your own gateway, AES-128, Class A/B/C, spreading factors SF7–SF12 trade rate for range) vs **NB-IoT/LTE-M** (licensed cellular, SIM-based) vs Sigfox. Put IoT devices on an isolated VLAN ([[Internet & Application Security#IoT Security|IoT Security]]). RF/Bluetooth attack tooling lives in [[06 Cybersecurity|Ethical Hacking]].

Related: [[Network Foundations]] · [[Network Types & Topologies]] · [[Wireless & Cellular]] · [[Space & Satellite Networks]] · [[05 Networking|domain overview]]
