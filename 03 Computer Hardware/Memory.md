---
type: note
tags: [hardware, memory]
---


The system's working storage — fast, volatile, and sitting between the CPU's [[CPU & Processing Units#2. Cache & the memory hierarchy|cache]] and persistent [[Storage]]. This note covers the memory taxonomy, how DRAM physically works, the SDRAM → DDR → LPDDR evolution, and the latch primitives that store a single bit.

> **Hierarchy (fast/small → slow/large):** registers → L1/L2/L3 cache (SRAM) → main memory (DRAM) → storage (SSD/HDD). Each level trades speed for capacity and cost.

---

## 1. Memory taxonomy

| Class | Keeps data without power? | Examples | Role |
|---|---|---|---|
| **Volatile** | No | CPU registers, SRAM cache, DRAM (DDR) | Active working data; lost on shutdown |
| **Non-volatile** | Yes | ROM, EEPROM, Flash, SSD/HDD | Boot firmware, long-term storage |

Inside a running process, memory is divided into **stack** (call frames, locals), **heap** (dynamic allocation), **data** (globals), and **code** segments, split across **user** and **kernel** address spaces. **Virtual memory** maps these to physical frames via **paging** (with a TLB cache for translations) and **swapping** to disk under pressure. *Security relevance:* most memory-corruption exploits (buffer/heap overflows) abuse the boundaries between these regions — see [[#7. Memory & security]].

---

## 2. How DRAM works

A DRAM cell is one **capacitor** (stores the bit as charge) + one **transistor** (access switch). Cheap and dense, but the charge leaks, so it must be **refreshed** thousands of times per second.

**Access is a 2-step grid lookup** (cells are arranged in rows × columns):

1. **RAS (Row Address Strobe)** — activate a row; its contents load into sense amplifiers (the row buffer).
2. **CAS (Column Address Strobe)** — select the column within that open row; data goes to the bus.

A repeat access to the *same open row* (row hit) needs only the CAS step — much faster. A different row must first be **precharged** before activation. **Bank interleaving** hides this latency by servicing another bank while one settles.

**Timing** is written `CL-tRCD-tRP-tRAS` (e.g. `16-18-18-36`):

| Param | Meaning |
|---|---|
| CL | Cycles from CAS to first data (CAS latency) |
| tRCD | Row-activate → column-access delay |
| tRP | Precharge time before opening a new row |
| tRAS | Minimum time a row stays active |

**Reliability & throughput add-ons:**
- **Parity / ECC** — ECC adds bits (72-bit module = 64 data + 8 ECC) to correct single-bit and detect double-bit errors (SECDED Hamming code). Required for servers; long ignored on consumer platforms.
- **Multi-channel** (dual/quad/octa) multiplies bandwidth by running DIMMs in parallel — e.g. DDR5-6400 dual-channel ≈ `6400 MT/s × 8 B × 2 ≈ 102 GB/s`.
- **Form factors:** **DIMM** (desktop/server), **SO-DIMM** (laptops) — same chips, smaller board.

---

## 3. SDRAM → DDR evolution

**SDRAM** synchronized DRAM to the bus clock (replacing asynchronous DRAM's unpredictable wait states), adding burst mode and the RAS/CAS/timing model still used today. PC100/PC133 were the last single-data-rate (SDR) standards — one transfer per clock edge.

**DDR (Double Data Rate)** transfers on **both** clock edges, doubling throughput at the same clock. Each generation lowers voltage, raises the prefetch width, and increases bandwidth:

| Gen | Year | Data rate | Voltage | Prefetch |
|---|---|---|---|---|
| DDR1 | 2000 | 200–400 MT/s | 2.5 V | 2n |
| DDR2 | 2003 | 400–1066 MT/s | 1.8 V | 4n |
| DDR3 | 2007 | 800–2133 MT/s | 1.5 V | 8n |
| DDR4 | 2014 | 2133–4266 MT/s | 1.2 V | 8n |
| DDR5 | 2020 | 4800–6400+ MT/s | 1.1 V | 16n |

> "DDR4-3200" = 3200 MT/s at a 1600 MHz clock. **MT/s** = mega-transfers/sec.

**DDR5 notable changes:** on-die ECC, two independent 32-bit sub-channels per DIMM, on-DIMM power management (PMIC), burst length 16. Kits ship at slow JEDEC defaults; enabling **XMP** (Intel) or **EXPO** (AMD) profiles in BIOS unlocks rated speed/timings.

---

## 4. LPDDR — mobile memory

Low-Power DDR optimizes for **energy, size, and heat** over raw capacity. It's soldered as **BGA** dies or stacked on the SoC (**Package-on-Package**), so it is not user-upgradeable but achieves very short, low-power signal paths.

| Gen | Max rate | I/O voltage note |
|---|---|---|
| LPDDR4 | 3200 MT/s | 1.1 V |
| LPDDR4X | 4266 MT/s | 0.6 V I/O (I/O power ∝ V², so ~75% less) |
| LPDDR5 | 6400 MT/s | 1.05 V |
| LPDDR5X | 8533 MT/s | 1.01 V |

**Unified Memory Architecture (Apple Silicon):** the LPDDR pool is shared by CPU, GPU, and Neural Engine, giving **zero-copy** between CPU and GPU (no PCIe VRAM transfer) and dynamic allocation. The M4 Max reaches ~546 GB/s — workstation-GPU territory — which is why UMA is compelling for on-device AI.

---

## 5. RDRAM — a historical cautionary tale

Rambus DRAM (late 1990s) bet on a **narrow 16-bit serial bus at very high frequency** (PC800 = 1.6 GB/s/channel) instead of a wide parallel one, using proprietary **RIMM** modules (empty slots needed C-RIMM blanks). Intel mandated it for the Pentium 4.

It lost decisively to DDR because it was **3–5× more expensive** (licensing baked in), had **higher latency**, ran **hot** (needed heat spreaders), and was **proprietary** while DDR evolved the open JEDEC standard that dozens of vendors could build cheaply. The lesson — technical peak performance on one axis loses to an open, "good-enough", affordable standard — recurs throughout tech history (Betamax/VHS, HD-DVD/Blu-ray).

---

## 6. SRAM and the latch (how a bit is physically stored)

**SRAM** stores each bit in a cross-coupled transistor pair (6T cell) — a **latch**. It needs no refresh and is very fast, so it's used for [[CPU & Processing Units#2. Cache & the memory hierarchy|cache]] and registers, but it's larger and costlier per bit than DRAM.

A **latch** is a bistable feedback circuit holding one bit. The family (full digital-logic treatment in [[Digital Logic & Microcontrollers]]):

| Type | Inputs | Behavior | Use |
|---|---|---|---|
| **SR** | S, R | Set / Reset / Hold; `S=R=1` forbidden | The foundational element |
| **D** | D, EN | Output follows D when enabled (transparent); no forbidden state | Registers, level hold |
| **D flip-flop** | D, CLK | Samples D only on the clock **edge** (master-slave) | Every synchronous register in a CPU |
| **JK** | J, K, CLK | Like SR but `J=K=1` **toggles** instead of being forbidden | Counters |
| **T** | T, CLK | `T=1` toggles every edge = divide-by-2 | Frequency dividers, ripple counters |

Key ideas: **level-sensitive** latches are transparent while enabled; **edge-triggered** flip-flops sample once per clock, which is why all synchronous logic is built from D flip-flops. Changing an input too close to the clock edge (violating **setup/hold** time) risks **metastability** — the cell hovers between 0 and 1 before resolving; synchronizer stages reduce the risk at asynchronous clock-domain crossings. A CPU register, the program counter, and status flags are all just banks of D flip-flops.

---

## 7. Memory & security

- **Stack/heap layout** is the battlefield for **buffer overflows**, use-after-free, and heap grooming.
- **Virtual memory + MMU** enforce process isolation (user vs kernel address space); defeating it (e.g. via page-table or DMA attacks) is a privilege-escalation goal.
- **Volatile-memory forensics** — RAM holds keys, clipboard, ARP/routing tables, and running-process artifacts; capturing it before shutdown is a core IR/forensics step.

Related: [[CPU & Processing Units]] · [[Storage]] · [[Digital Logic & Microcontrollers]] · [[Motherboard & IO]] · [[03 Computer Hardware|domain overview]]
