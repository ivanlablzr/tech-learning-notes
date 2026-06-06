---
type: note
tags: [electronics, semiconductors, ics]
---


**Active** components require power and can **amplify or switch** a signal (unlike [[Passive Components|passives]]). In modern electronics this means **semiconductors** — diodes, transistors, and the integrated circuits built from them.

> *Historical/other actives:* vacuum tubes (triode→pentode, magnetron, klystron, CRT) ran early electronics and radar; gas-discharge devices (thyratron, ignitron) switched high power; "power sources" (batteries, fuel/solar cells, generators) supply rather than process signals. All are niche or obsolete today — semiconductors won.

---

## 1. Diodes — one-way current valves

Conduct in one direction; the workhorse is the **rectifier** (AC→DC in power supplies). Key types:

| Diode | Special property | Use |
|---|---|---|
| **Rectifier / bridge** | Forward conduct only | AC→DC conversion |
| **Schottky** | Very fast, low forward drop | High-frequency, low-loss |
| **Zener** | Conducts in *reverse* at a set voltage | Voltage reference/regulation |
| **TVS** | Absorbs voltage spikes | Surge protection |
| **LED / laser** | Emits light when forward-biased | Indicators, displays, optics |
| **Photodiode / solar cell** | Current ∝ incident light | Sensing, power generation |
| **Varactor** | Capacitance varies with voltage | Tuning circuits |

## 2. Transistors — amplify & switch

Two great families:

- **BJT (Bipolar Junction Transistor)** — **current-controlled** (base current controls collector current); NPN/PNP. Good linear amplifiers; variants: Darlington (high gain), phototransistor.
- **FET (Field-Effect Transistor)** — **voltage-controlled**; the **MOSFET** dominates (see below). Others: JFET, MESFET, HEMT (RF/microwave), FinFET/multi-gate (modern CPUs).
- **Thyristors** (SCR, TRIAC) — latching switches for AC power control.

### MOSFET — the most-manufactured device on Earth

Voltage on an insulated **gate** controls current between **source** and **drain**; gate draws ~zero steady current. Nearly all digital logic is CMOS MOSFETs.

- **N-channel (NMOS)** conducts electrons — faster, lower on-resistance; **P-channel (PMOS)** conducts holes. **CMOS** pairs them so only one is ever on → near-zero static power (NMOS pulls output low, PMOS pulls high).
- **Enhancement mode** (default OFF, needs V_GS > threshold) is standard for switching; depletion mode (default ON) is rare.
- **Operating regions:** *cutoff* (off), *linear/ohmic* (acts as a voltage-controlled resistor — where a switch should sit, lowest loss), *saturation* (constant current — where amplifiers sit).

**Key parameters:** threshold `V_th`, on-resistance `R_DS(on)`, max drain current `I_D`, breakdown `V_DS(max)`, gate charge `Q_g`. The core trade-off: higher voltage rating ⇒ higher `R_DS(on)` for the same die area.

**As a switch** (the common embedded use — driving loads beyond a GPIO's current): a **low-side NMOS** is simplest (GPIO drives the gate); use a **logic-level** MOSFET so 3.3/5 V fully turns it on. Power switching needs a **gate driver** + series gate resistor; H-bridges drive motors both directions but must avoid **shoot-through** (never both transistors on one leg). Losses: `P ≈ I²·R_DS(on)·D (conduction) + ½·V·I·(t_r+t_f)·f (switching)` → heatsinking matters.

**Wide-bandgap (next-gen):** **SiC** (high voltage/temperature — EV and solar inverters) and **GaN** (very fast switching — USB-C "GaN" chargers, RF). Common parts: IRLZ44N (logic-level 55 V/47 A), IRF540N (power), AO3400 (SMD).

## 3. Optoelectronics

Light ↔ electricity devices: **LEDs/laser diodes** (emit), **photodiodes/phototransistors** (detect), **opto-isolators/opto-couplers** (transfer a signal across an electrical barrier for isolation), and **LED displays** (7-segment, dot-matrix).

## 4. Integrated Circuits (ICs)

Millions–billions of transistors fabricated together on one silicon die. ICs win on **size, speed, power, cost, and reliability** vs discrete circuits — an Apple M4 packs ~28 billion transistors on a 3 nm node. **Moore's Law** (transistor count doubling ~every 2 years) held ~50 years and is now slowing.

**By signal type:**
- **Analog** — op-amps (LM741/TL071), voltage regulators (LM7805 fixed, LM317 adjustable), comparators, the **NE555** timer, ADC/DAC.
- **Digital** — logic gates (74HCxx CMOS), flip-flops/shift registers (74HC595), counters, memory (SRAM/Flash/EEPROM), and [[Digital Logic & Microcontrollers|microcontrollers]]/FPGAs.
- **Mixed-signal** — ADC/DAC, motor drivers (L298N H-bridge), RF transceivers (nRF24L01), power-management ICs (PMIC).

**By integration scale:** SSI (<100) → MSI → LSI → VLSI (100k–1B) → **ULSI** (>1B; modern CPUs/GPUs/SoCs).

**By technology:** **CMOS** dominates digital (low static power); legacy **TTL/bipolar** (faster but power-hungry); **BiCMOS** mixes both; **3D ICs** stack dies via through-silicon vias (HBM memory stacks, **chiplets** like AMD Ryzen, Apple-M SoCs).

**Fabrication (outline):** grow silicon ingot → slice 300 mm wafers → repeated cycles of **photolithography → etch → dope (ion implant) → deposit → planarize** for ~100+ layers → copper metallization → test → dice & package. Leading foundries: **TSMC**, Samsung, Intel — all dependent on **ASML's EUV** machines (see [[Hardware Logic & Fabrication#2. Photolithography — printing the chip|photolithography]]). Node names ("5 nm", "3 nm", "2 nm") are marketing, not literal gate length.

**Packaging:** through-hole **DIP** (breadboard-friendly) → surface-mount **SOIC/QFP/QFN** → **BGA** (solder balls, high pin count: CPUs/FPGAs) → **SiP** (multiple dies in one package).

**Using a chip — the datasheet** tells you everything: pinout, **absolute maximum ratings** (never exceed), recommended operating conditions, electrical characteristics, and logic-level thresholds (**V_IH/V_IL** inputs, **V_OH/V_OL** outputs) — essential for safely interfacing parts at different voltages.

## 5. Programmable logic

**FPGA** (field-programmable gate array) and **CPLD** — chips whose digital logic you configure after manufacture (hardware described in HDL), bridging fixed ICs and custom ASICs. Used for prototyping, low-latency signal processing, and networking ([[CPU & Processing Units#3.3 NPU (Network Processing Unit)|Tofino NPUs]] are P4-programmable).

Related: [[Passive Components]] · [[Digital Logic & Microcontrollers]] · [[Hardware Logic & Fabrication]] · [[02 Electronics|domain overview]]
