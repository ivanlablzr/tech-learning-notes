---
type: note
tags: [networking, space, satellites]
---


Networking in the most hostile environment there is — where you can't fix hardware, latency is measured in minutes, and radiation flips bits. Increasingly critical infrastructure as commercial space booms.

## 1. Orbits

| Orbit | Altitude | Latency | Use |
|---|---|---|---|
| **LEO** | 160–2,000 km | ~20–40 ms | broadband constellations (Starlink), observation, ISS — needs many satellites |
| **MEO** | 2,000–35,786 km | ~100–150 ms | **navigation** (GPS, Galileo); crosses Van Allen belts |
| **GEO/GSO** | 35,786 km | ~600 ms | fixed over a point — comms, weather; 3 cover most of Earth |
| **HEO** | varies | varies | long dwell over high latitudes (Molniya — Russia/early warning) |
| **Polar / SSO** | ~500–900 km | LEO | Earth observation with consistent lighting; sees whole planet |

**Satellite types** by function: **communication** (relay/amplify — Intelsat, Iridium), **navigation** (GPS/GLONASS/Galileo/BeiDou), **Earth observation** (Landsat, Sentinel), **weather** (GOES), **scientific** (Hubble, JWST), **military/recon** (Keyhole), **exploration** (Voyager), **tech-demo** (CubeSats). Segments: **space** (satellite + TT&C) and **ground** (gateways, VSAT, user terminals).

## 2. Space engineering constraints

No maintenance, vacuum (no convective cooling), ±150 °C swings every orbit, launch vibration, and minutes-long comms delay. Consequences:

- **Radiation hardening:** solar particles, cosmic rays, and Van Allen belts cause **SEUs** (bit-flips — mitigated by EDAC + memory scrubbing), **Total Ionizing Dose** (cumulative transistor degradation — rad-hard parts rated 100–300+ krad vs commercial 1–10), and **latch-up** (destructive CMOS short — current limiters). Approaches: rad-hard-by-design/process (SOI), or shielded **COTS** for cheap LEO smallsats.
- **Redundancy:** **Triple Modular Redundancy** (three units vote), hot/cold spares, watchdog timers.
- **Thermal:** heat pipes, radiators on the shadow face, multilayer insulation, heaters.
- **Power:** solar panels (degrade ~1–2%/yr) + Li-ion batteries for eclipse; **RTGs** (Pu-238 decay) for deep space where the sun is too weak (Voyager, Perseverance).

## 3. Satellite communications

- **Transponders** receive an uplink, shift frequency, amplify, and retransmit downlink — **bent-pipe** (no processing) vs **regenerative** (demod/re-encode, enables onboard routing). Uplink and downlink use different frequencies.

| Band | GHz | Use |
|---|---|---|
| L | 1–2 | GPS, Iridium mobile |
| C | 4–8 | broadcast (rain-fade resistant) |
| Ku | 12–18 | TV, Starlink gen-1 |
| Ka | 26.5–40 | high-throughput, Starlink |

Higher band = more bandwidth but more **rain fade**. A **link budget** (EIRP, free-space path loss, G/T, Eb/N0, link margin) determines whether a link closes.

## 4. Ground segment

Every satellite needs **TT&C** (Telemetry — health data down; Tracking — orbit determination; Command — instructions up). Networks: NASA **DSN** (three 120°-spaced sites for continuous deep-space coverage — now congested), ESA ESTRACK, and **ground-station-as-a-service** (AWS Ground Station, Azure Orbital). **VSAT** = small enterprise/consumer dishes for maritime/aviation/remote broadband.

## 5. Launch & smallsats

| Vehicle | Operator | To LEO |
|---|---|---|
| Falcon 9 (reusable) | SpaceX | ~22.8 t |
| Falcon Heavy | SpaceX | ~63.8 t |
| Starship | SpaceX | >100 t (target) |
| Ariane 6 | ESA | ~21 t |
| Electron | Rocket Lab | ~0.3 t |

**Reusability** dropped $/kg to LEO from ~$50k to ~$2.7k. **CubeSats** (1U = 10 cm³, ~1.3 kg, rideshare-deployed) put satellites within reach of universities and startups.

## 6. Space networking protocols

Standard TCP/IP fails in space (minute-long RTTs break timeouts; links are intermittent). So:
- **CCSDS** standards (NASA/ESA/JAXA): TM/TC framing, **CFDP** (reliable file delivery), AOS.
- **DTN (Delay-Tolerant Networking)** — store-and-forward: the **Bundle Protocol** (BP7, RFC 9171) forwards "bundles" hop-by-hop with **custody transfer**; **LTP** is its transport for high-latency links. The basis of the **interplanetary internet** (championed by Vint Cerf; deployed on ISS, tested at Mars). **LunaNet** extends this to the Moon (~1.3 s one-way).

## 7. Constellations & security

**LEO broadband constellations:** **Starlink** (6,000+ sats @ ~550 km, 20–40 ms, Starshield military variant with laser inter-satellite links), **OneWeb** (~648 @ 1,200 km, enterprise/gov), **Amazon Kuiper** (planned 3,236). **O3b/SES** runs MEO for low-latency high-throughput.

**Space cybersecurity** attack surface: **jamming** (RF interference — rife in conflict zones), **GPS/AIS spoofing** (false position), **uplink hijacking**, **ground-station compromise** (often the weakest link — standard IT security applies), supply-chain, and eavesdropping on unencrypted downlinks.

> **Case study — Viasat KA-SAT (24 Feb 2022):** as Russia invaded Ukraine, **AcidRain** wiper malware pushed via a misconfigured VPN bricked ~40,000 modems across Europe, disrupting Ukrainian military comms. Attributed to Russia's GRU — a textbook attack on space as critical infrastructure.

**Defenses:** mandatory encrypted + authenticated command links, frequency-hopping/spread-spectrum (anti-jam), anomaly monitoring, hardened ground stations, anti-spoof (dual-frequency, signal authentication). **Emerging:** in-orbit servicing (Northrop MEV), **debris/Kessler syndrome** mitigation (FCC 5-year deorbit rule, active removal).

Related: [[Network Foundations]] · [[Network Media & Links]] · [[Wireless & Cellular]] · [[Network Infrastructure]] · [[05 Networking|domain overview]]
