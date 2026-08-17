#networking #technology #cybersecurity

> [[05 Networking]]. From your [[Mission Priority]]: *"What protocols are used in different domains — industry, military, healthcare, space…?"* (Also directly useful for your [[06 Cybersecurity|cyber]] career: each sector = a different attack surface.)

The internet protocols you know (TCP/IP, HTTP, DNS — see [[OSI Layers & Protocols]]) are only the general-purpose layer. **Each critical sector runs its own specialised protocols**, often old, often insecure, and increasingly targeted. Knowing them is a real edge in [[06 Cybersecurity|cybersecurity]].

## Industrial / OT (operational technology)
The protocols that run factories, power grids, water plants — **ICS/SCADA**:
- **Modbus** (1979, no built-in security), **DNP3**, **PROFINET**, **EtherCAT**, **OPC-UA** (modern, secure-capable).
- Why it matters: these control physical infrastructure; attacks here (Stuxnet, grid hacks) are [[Geopolitics|geopolitical]] weapons.

## Healthcare
- **HL7** and **FHIR** — exchanging medical records.
- **DICOM** — medical imaging (X-ray, MRI).
- High-value target: patient data + life-critical devices.

## Military / defence
- **Link 16 / TADIL** — tactical data links between aircraft/ships.
- **MIL-STD-1553** — avionics bus.
- Heavy use of **encryption**, frequency hopping, and hardened/air-gapped networks.

## Aerospace / space
- **CCSDS** — international standard for spacecraft–ground comms.
- **SpaceWire** — onboard spacecraft networks.
- Deep-space adds huge latency → **Delay-Tolerant Networking (DTN)**. Ties to [[Space & Satellite Networks]], [[Orbits]].

## Automotive
- **CAN bus** — the network inside your car (engine, brakes, sensors). Famously insecure → car-hacking research.

## Finance
- **FIX** — trading messages between institutions ([[Trading]]).
- **ISO 8583** — card/ATM transactions; **SWIFT** — interbank messaging.

## IoT / smart devices
- **MQTT**, **CoAP**, **Zigbee**, **Z-Wave**, **LoRaWAN** — lightweight protocols for low-power devices.

## The security throughline
Most of these were designed for *isolated* networks and assume trust → when connected to IP networks they become soft targets. This is exactly where [[Ethical Hacking|OT/ICS security]] is a growing, high-value specialty for your career.

## Connects to
[[05 Networking]] · [[OSI Layers & Protocols]] · [[06 Cybersecurity]] · [[Ethical Hacking]] · [[Space & Satellite Networks]] · [[Geopolitics]] (critical infrastructure).
