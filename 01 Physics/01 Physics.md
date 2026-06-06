---
type: domain
tags: [domain, moc]
aliases: [Physics]
---


> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

Computing is physics made useful: electrons in silicon, photons in fiber, electromagnetic waves in the air.

> [!question] The five questions
> **Why it exists:** to describe how energy, charge, and waves behave in the physical world.
> **Problem it solves:** explains *why* transistors switch, signals attenuate, and wireless works.
> **Depends on:** [[00 Mathematics#Overview|Mathematics]] (calculus, vectors).
> **Depended on by:** [[02 Electronics|Electronics]], [[05 Networking|Networking]] (RF, fiber).
> **Interactions:** electricity → components; electromagnetism → Ethernet/Wi-Fi/cellular; wave theory → fiber & RF; information theory → channel limits.

### Sub-areas
- **Electricity** — voltage, current, resistance, power (V=IR).
- **Electromagnetism** — electric/magnetic fields, induction → all wired/wireless comms.
- **Semiconductor physics** — why doped silicon can switch → the [[02 Electronics|transistor]].
- **Wave theory** — frequency, amplitude, wavelength, phase → RF & fiber optics.
- **Information theory** (shared with [[00 Mathematics#Overview|Math]]) — entropy, noise, channel capacity.

See [[Knowledge Graph#Domain Dependency Graph]].

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[Electrical Engineering & Electricity]], [[Network Media & Links|Radio Frequency (RF)]], [[Passive Components|Antennas]], [[Network Media & Links|Optical Fiber cables]].

## Learning Path

**Prerequisites:** [[00 Mathematics#Overview|Mathematics]] (calculus, vectors).

1. Electricity — charge, voltage, current, resistance, power; Ohm's & Kirchhoff's laws.
2. Circuits — series/parallel, RC/RL behavior.
3. Electromagnetism — fields, induction, EM spectrum.
4. Semiconductor physics — bands, doping, the PN junction (→ diode/transistor).
5. Wave theory — modulation, attenuation, reflection, multipath.
6. Information theory — entropy, noise, Shannon capacity.

**Milestones:** explain *why* a transistor switches; relate Wi-Fi range to frequency/attenuation (supports networking lab).

## Dependencies

**Depends on:** [[00 Mathematics#Overview|Mathematics]].

**Depended on by:**
- [[02 Electronics|Electronics]] — semiconductor physics underlies every component.
- [[03 Computer Hardware#Overview|Hardware]] — heat, power, signal integrity.
- [[05 Networking|Networking]] — RF, fiber, signal propagation (physical layer).

```text
Mathematics → Physics → (Electronics → Hardware, Networking physical layer)
```

## Bridge Topics

- **Semiconductor physics ↔ Transistor** — physics becomes a switch. Owner: [[02 Electronics|Bridge Topics]].
- **Electromagnetism/wave theory ↔ Physical layer** — physics becomes Ethernet/Wi-Fi/fiber. Owner: [[05 Networking|Bridge Topics]].
- **Information theory ↔ Channel capacity** — physics+math bound how fast a link can be.

Canonical bridge notes: [[Knowledge Graph#Bridge Topic Map]].

## Projects

- **Breadboard logic** — build an AND/OR from transistors; feel gates physically (supports L0 [[Learning Paths & Projects#Project Roadmap|Logic-to-CPU]]).
- **RF range experiment** — measure Wi-Fi signal vs distance/obstacles (supports Home networking lab).
- **Simple radio / antenna** — modulation & wave theory hands-on.

See [[Learning Paths & Projects#Project Roadmap]].
