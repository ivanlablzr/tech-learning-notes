---
type: note
tags: [electronics, electrical-engineering, power]
---


The physics of electric charge and its application — from a single resistor to the power grid. (Individual components live in [[Passive Components]] and [[Active Components & ICs]]; this note covers circuit theory, AC/DC, power systems, motors, and power electronics.)

## 1. Fundamentals

Electricity is energy from the movement of charge — **static** (stationary buildup) or **current** (flowing electrons). Conventional current is defined +→−. Like charges repel; **Coulomb's law** `F = k·q₁q₂/r²`.

| Quantity | Symbol | Unit | Relation |
|---|---|---|---|
| Charge | Q | Coulomb | — |
| Current | I | Ampere | I = Q/t |
| Voltage | V | Volt | V = W/Q |
| Resistance | R | Ohm | — |
| Power | P | Watt | P = W/t |

- **DC** — constant direction (batteries, solar, electronics).
- **AC** — reverses periodically; grid is 50 Hz (most of world) or 60 Hz (N. America/Japan).

## 2. Circuit theory

**Ohm's law `V = I·R`**, and power `P = VI = I²R = V²/R`.

| | Series | Parallel |
|---|---|---|
| Resistance | R = ΣRₙ | 1/R = Σ(1/Rₙ) |
| Current | same through all | divides |
| Voltage | divides | same across all |
| One fails | whole circuit opens | others keep working |

- **Kirchhoff's laws:** **KVL** — voltages around any loop sum to 0 (energy conservation); **KCL** — current into a node = current out (charge conservation).
- **Analysis tools:** node-voltage and mesh-current methods; **Thévenin/Norton** (reduce any linear network to one source + one resistance); **superposition** (sum each source's effect).
- **Reactive elements:** capacitor stores energy in a field (`W=½CV²`, `τ=RC`); inductor stores energy in a magnetic field (`V=L·dI/dt`, `W=½LI²`, `τ=L/R`). **Transformers** change AC voltage by turns ratio `V_p/V_s = N_p/N_s` (step-up trades voltage for current).

## 3. AC vs DC

- **AC waveform:** peak, **RMS** (`V_rms = V_peak/√2`; 230 V mains ≈ 325 V peak), frequency, phase. **Harmonics** (integer multiples) degrade power quality.
- **Why AC for the grid:** transformers only work on AC, enabling high-voltage transmission that minimizes `I²R` loss.
- **Rectification (AC→DC):** half-wave (1 diode, ~40% eff.) vs **full-wave bridge** (4 diodes, ~81%); smooth with a capacitor, stabilize with a regulator. A linear supply chains transformer → rectifier → filter cap → regulator → protection.

## 4. Power systems

**Generation** turns a turbine to spin a generator (Faraday's law: a changing magnetic field induces EMF). Sources: thermal (coal/gas/nuclear), hydro, wind, solar PV (DC → needs inverter), and storage-based (e.g. CO₂/compressed-air).

**Transmission & distribution** raises voltage for transport, then steps it down:
```
Gen (11–25 kV) → step-up (100–765 kV) → transmission → step-down → distribution (11 kV) → 230/120 V → consumer
```
High voltage cuts losses (`P_loss=I²R`; doubling V quarters I → 16× less loss). **Three-phase** (conductors 120° apart) is more efficient for transmission and motors. Grid **frequency** must stay at 50/60 Hz — deviation signals supply/demand imbalance.

**Safety:** grounding/earthing gives fault current a safe path; protective devices = **fuse** (one-shot), **circuit breaker**, **GFCI/RCD** (trips on ~4–6 mA leakage — anti-electrocution), **AFCI** (arc faults). Codes: IEC 60364, US NEC. *Lethal:* ~10 mA through the heart can cause fibrillation — de-energize and verify before working.

**Smart grid:** smart meters (AMI), **SCADA** control, demand response, vehicle-to-grid (V2G), and islanding **microgrids**.

## 5. Motors & machines

- **Induction (AC) motor** — dominant in industry; a rotating stator field drags the rotor (with **slip**); synchronous speed `Nₛ = 120f/P`; 85–97% efficient; started DOL/star-delta/soft-starter/VFD.
- **Synchronous machine** — rotor locked to line frequency; power-plant **alternators** and power-factor correction.
- **DC motor** — armature current in a field makes force (`F=BIL`); speed via voltage; BLDC is the modern brushless form.
- **Control:** **VFD** (vary frequency → speed, big energy saver), **FOC** (vector control), **PID** feedback loops.

## 6. Power electronics & control

Switching devices: diode (rectify), **SCR/TRIAC** (latching AC control), **power MOSFET** (fast, SMPS/inverters), **IGBT** (high-voltage motor drives). Converters:

- **AC→DC** rectifiers (uncontrolled diode bridge or controlled SCR).
- **DC→DC** switching regulators — **buck** (step-down), **boost** (step-up), **buck-boost** — 85–99% efficient vs ~30–50% for linear; the basis of **SMPS**.
- **DC→AC** **inverters** (H-bridge + PWM) — solar grid-tie, UPS, EV powertrains.

**Industrial control:** **PLCs** (ladder logic, scan cycle read→execute→write; Modbus/PROFINET; Siemens/Allen-Bradley), **SCADA/HMI**, and robots/cobots/IIoT.

## 7. Advanced & emerging

- **High voltage:** breakdown, corona loss, surge arresters; **HVDC** for long submarine/asynchronous links.
- **Power quality:** harmonics (3rd/5th/7th from non-linear loads) overheat neutrals; **power factor** `PF = P/S = cos φ`, corrected with capacitor banks.
- **Energy storage:** Li-ion/LFP (EVs, grid), flow batteries (long-duration), flywheels/supercaps (high power, fast), pumped hydro (largest grid-scale).
- **EMC:** manage **EMI** (conducted/radiated) via shielding (Faraday cage), filtering (ferrite beads, common-mode chokes), grounding, and PCB layout.
- **Frontier:** wide-bandgap **SiC/GaN**, wireless power (Qi, EV resonant), solid-state transformers, grid-forming inverters, superconductivity.

> **Lab tools:** multimeter (V in parallel, I in series, R de-energized), oscilloscope (voltage vs time), function generator, clamp meter. Simulate with **LTspice**; design PCBs with **KiCad**.

Related: [[Passive Components]] · [[Active Components & ICs]] · [[01 Physics]] · [[02 Electronics|domain overview]]
