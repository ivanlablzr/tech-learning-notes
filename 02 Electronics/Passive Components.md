---
type: note
tags: [electronics, components, passive]
---



**Passive** components need no external power source and **cannot amplify** a signal — they only store, release, dissipate, or filter energy. (Components that *can* amplify or switch a signal — diodes, transistors, ICs — are **active**; see [[Active Components & ICs]].)

The three fundamental passives are the **resistor, capacitor, and inductor**; the memristor is sometimes called the "fourth." Everything below is built from or around them.

## Resistors — oppose current, dissipate energy as heat

Set by `V = I·R`. Variants:

- **Fixed** — standard, plus **power resistors** (large, to shed heat) and **resistor networks/arrays** (SIP/DIP packages).
- **Variable** — **rheostat** (2-terminal), **potentiometer** (3-terminal voltage divider), **trim pot** (internal tweaks).
- **Sensing (resistance changes with environment)** — **thermistor** (temperature, PTC/NTC), **photoresistor/LDR** (light), **humistor** (humidity), **varistor/MOV** (clamps over-voltage).
- **Resistance wire / heating elements** (e.g. Nichrome).

## Capacitors — store charge in an electric field

Block DC while passing AC; used for power-supply filtering, decoupling, timing, and tuning. Common families:

- **Ceramic** — small, cheap, ubiquitous decoupling/bypass.
- **Film** — stable, low-loss, for signal and power circuits.
- **Electrolytic** (aluminum, tantalum, polymer) — high capacitance, polarized; bulk/reservoir filtering.
- **Supercapacitor** — very high capacitance for energy buffering/backup.
- **Variable** (tuning, trimmer) — adjustable, for radios and tuned circuits.

> By *role* rather than construction you'll hear: **decoupling/bypass** (clean a supply rail), **coupling** (pass AC, block DC between stages), **reservoir/bulk** (smooth a rectified supply), **filter**, **snubber**.

## Inductors & magnetic devices — store energy in a magnetic field

A coil that resists changes in current. Family includes **inductors/chokes**, **variable** and **saturable** inductors, **transformers** (couple/step AC voltages), **ferrite beads** (suppress high-frequency noise), and electromechanical cousins like **solenoids, motors, loudspeakers, and microphones** that convert between electrical and magnetic/mechanical energy.

## Memristor

The "memory + resistor": passes charge in proportion to magnetic flux and **retains its previous resistance state**, making it interesting for non-volatile memory and analog/neuromorphic computing. Still largely emerging in practice.

## Passive networks, transducers & supporting parts

- **RC / LC networks** — combinations forming filters, snubbers, and tuned/resonant circuits.
- **Transducers & sensors** (passive) — convert between physical quantities and electrical signals: thermocouples/RTDs (temperature), strain gauges and accelerometers (force/motion), LVDTs and encoders (position), photoresistors (light), magnetometers (field), microphones/buzzers (audio). *(Motors, relays, and solenoids are actuators — see [[Electromechanical Components]].)*
- **Integrated passive devices (IPDs)** — multiple passives in one package, saving board space.
- **Antennas** — transmit/receive radio waves; types include dipole, Yagi, phased array, loop, and parabolic dish (deep dive in [[01 Physics]]/networking RF).
- **Prototyping aids** — breadboards and wire-wrap for building circuits without soldering.

Related: [[Active Components & ICs]] · [[Digital Logic & Microcontrollers]] · [[Electrical Engineering & Electricity]] · [[02 Electronics|domain overview]]
