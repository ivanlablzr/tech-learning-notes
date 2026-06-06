---
type: note
tags: [hardware, storage]
---


**Persistent (non-volatile)** data — survives power-off, unlike [[Memory|RAM]]. The trade-offs across media are capacity vs. speed vs. endurance vs. cost.

## HDD — Hard Disk Drive

Magnetic **platters** spinning at a fixed RPM, with read/write heads on an actuator arm floating nanometers above the surface. Cheap and high-capacity, but mechanical — slow random I/O and shock-sensitive.

| RPM | Use | Seq. read | Seek |
|---|---|---|---|
| 5400 | Laptop / NAS | ~100–120 MB/s | ~12 ms |
| 7200 | Desktop | ~150–200 MB/s | ~8 ms |
| 10–15k | Enterprise SAS | ~200–260 MB/s | 2–5 ms |

Interfaces: **SATA** (6 Gbps, consumer) and **SAS** (12 Gbps, dual-ported, enterprise). Strengths: cost (~$15–30/TB), capacity (up to ~24 TB), sequential throughput, write endurance. Weaknesses: seek latency, fragility, noise, power.

## SSD — Solid State Drive

Data held in **NAND flash** (charge-trap transistors) — no moving parts, so very low latency and high IOPS. Cells wear out with each program/erase cycle, so controllers use **wear leveling**.

| NAND | Bits/cell | Endurance | Trade-off |
|---|---|---|---|
| SLC | 1 | ~100k P/E | Fastest, priciest (enterprise) |
| MLC | 2 | ~10k | Balanced |
| TLC | 3 | ~3k | Mainstream consumer |
| QLC | 4 | ~1k | Cheapest, densest, slowest |

**3D NAND** stacks cells vertically for density without shrinking geometry. Endurance is rated as **TBW** (terabytes written over life) and **DWPD** (drive writes per day over warranty).

## NVMe — removing the SATA bottleneck

SATA's AHCI protocol was built for mechanical disks (queue depth 32, ~550 MB/s ceiling). **NVMe** talks directly over **PCIe** with massive parallelism (65,535 queues) and far lower latency.

| Interface | Max seq. read | Latency |
|---|---|---|
| SATA III (AHCI) | ~550 MB/s | ~100 µs |
| PCIe 3.0 ×4 NVMe | ~3,500 MB/s | ~25 µs |
| PCIe 4.0 ×4 NVMe | ~7,000 MB/s | ~20 µs |
| PCIe 5.0 ×4 NVMe | ~14,000 MB/s | ~15 µs |

**M.2** is the common board form factor (22 mm wide); keying (M vs B+M) and host wiring decide whether a slot runs SATA, NVMe, or both.

## RAID — combining drives

A controller presents multiple drives as one logical volume for redundancy and/or speed.

| Level | Min | Method | Survives | Capacity |
|---|---|---|---|---|
| 0 | 2 | Striping | nothing | 100% |
| 1 | 2 | Mirroring | 1 fail | 50% |
| 5 | 3 | Stripe + parity | 1 fail | (N−1)/N |
| 6 | 4 | Stripe + dual parity | 2 fails | (N−2)/N |
| 10 | 4 | Mirror + stripe | 1 per mirror | 50% |

> **RAID is not a backup.** It survives drive failure, not deletion, ransomware, fire, or theft. Keep separate backups (3-2-1).

## Emerging & tiering

- **Intel Optane / 3D XPoint** (discontinued 2022–23): byte-addressable, ~10 µs latency, ~1000× NAND endurance — killed by cost-per-GB vs advancing 3D NAND.
- **Local vs cloud:** local NVMe = sub-ms latency, GB/s, full control; cloud ([[Cloud & Datacenters|S3/Blob]]) = network latency but provider-managed durability and elastic cost. Common hybrid: hot data on NVMe, cold/archive in object storage (e.g. Glacier ~$0.001/GB/mo).

Related: [[Memory]] · [[Motherboard & IO]] · [[CPU & Processing Units]] · [[Network Performance & Resilience|RAID in storage arrays]]
