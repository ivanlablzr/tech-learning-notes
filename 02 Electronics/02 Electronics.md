---
type: domain
tags: [domain, moc, electronics]
---


Where physics becomes controllable — components and logic that turn voltage into computation. Electronics controls the flow of electrons to **process information, convert power, sense the world, and drive machines**.

> *Electronics vs Electrical Engineering:* [[Electrical Engineering & Electricity|Electrical engineering]] handles power generation, transmission, and large machines; **electronics** focuses on low-power semiconductor devices and signal/information processing. They meet in power electronics.

## Domain notes

| Note | Covers |
|---|---|
| [[Passive Components]] | Resistors, capacitors, inductors, sensors, antennas |
| [[Active Components & ICs]] | Diodes, transistors/MOSFET, ICs, fabrication, FPGAs |
| [[Electromechanical Components]] | Switches, relays, connectors, protection, piezo, MEMS |
| [[Digital Logic & Microcontrollers]] | Gates → memory → ADC/DAC; MCUs and embedded interfacing |
| [[Electrical Engineering & Electricity]] | Circuit theory, AC/DC, power systems, motors, power electronics |

## The five views

**Why it exists / what it solves:** to harness semiconductor physics into reliable switching and signal processing — building deterministic 0/1 logic out of messy analog reality.

**Dependencies:** builds on [[01 Physics]] (semiconductors) and [[00 Mathematics]] (boolean algebra); is depended on by [[03 Computer Hardware]] (gates and sequential logic become ALUs, registers, memory).
```
Physics + Boolean algebra → Electronics → Computer Hardware
```

**Bridge topic it owns — the transistor:** the exact point where semiconductor physics becomes a controllable switch. Everything digital stands on it (see [[Knowledge Graph#Bridge Topic Map]]).

**Learning path:** passive components & circuits → diodes/transistors (MOSFET as a switch) → logic gates (NAND is universal) → combinational logic (adders, MUX) → sequential logic (flip-flops, registers, counters) → microcontrollers.

**Projects:** transistor logic gates on a breadboard → a 4-bit adder from gates → a **Logic-to-CPU simulator** (Logisim/nand2tetris) — the L0 anchor in [[Learning Paths & Projects#Project Roadmap]].

## Fundamentals & bench practice

The core relationships (full treatment in [[Electrical Engineering & Electricity]]): **Ohm's law `V=I·R`**, **power `P=VI=I²R=V²/R`**, and Kirchhoff's **KVL/KCL** for analysis. Components combine in **series** (same current, voltages/resistances add) or **parallel** (same voltage, currents add, `1/R=Σ1/Rₙ`).

**Signals:**
- **Analog** — continuous, full real-world resolution, but noise-prone.
- **Digital** — two states defined by logic thresholds; noise-immune within margins, easy to store/process (at the cost of ADC quantization).
- Quality is judged by **SNR** (signal/noise, in dB) and **bandwidth** (frequency range = information rate). **Reactance** makes capacitors pass high frequencies (`Xc=1/2πfC`) and inductors block them (`X_L=2πfL`).

**Reading schematics:** components carry reference designators (R, C, U=IC, Q=transistor, D=diode, L=inductor); a dot at a crossing = connected; VCC/GND symbols are implied rails. Symbols follow IEC or ANSI conventions ([[Electronic symbol]]).

**Bench tools:** **multimeter** (V in parallel, I in series, R de-energized), **oscilloscope** (waveforms over time — the key debug tool, use a 10× probe), **logic analyzer** (decode UART/SPI/I²C), function generator, current-limited bench supply, soldering iron (~300–370 °C, heat the joint not the solder). Simulate in **LTspice**/Falstad; lay out PCBs in **KiCad**.

> [!warning] Safety
> Voltages above ~50 V AC / 120 V DC can be lethal — mains kills reliably. **ESD**: thousands of volts of body static can silently destroy ICs/MOSFETs — use a grounded wrist strap. **Capacitors** (power supplies, camera flashes, CRTs) store lethal charge after power-off — discharge through a resistor first. Check electrolytic/diode polarity before powering; fuse against shorts.

Related: [[01 Physics]] · [[03 Computer Hardware]] · [[Knowledge Graph]] · [[Home]]
