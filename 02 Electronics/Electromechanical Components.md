---
type: note
tags: [electronics, components, electromechanical]
---



Components that **bridge the electrical and mechanical worlds** — converting electrical energy into physical motion (or vice versa), or using mechanical action to make/break a circuit.

## Switches & relays — open or close a circuit

- **Manual switches** — described by pole/throw count (**SPST, SPDT, DPDT**…) and form (toggle, rocker, rotary, pushbutton, slide). **DIP switches** set internal configuration; **keypads** are switch arrays.
- **Sensing switches** — actuate on a physical condition: **micro/limit** (motion), **reed** (magnetic), **thermostat** (heat), **mercury/centrifugal** (tilt/rotation).
- **Relays & contactors** — a small electrical signal electromechanically switches a larger circuit (solid-state relays do this without moving parts).
- **Disconnectors / transfer switches** — isolate or switch loads between sources, used in power distribution.

## Connectors & terminals — make/break electrical connections

Terminals, screw/terminal blocks, pin headers, and **connectors** (jacks/sockets, board-to-board, cable assemblies such as power cords and patch cords). The mechanical reliability of connectors is a common real-world failure point.

## Protection devices — guard against over-current/voltage

- **Over-current:** **fuse** (one-shot), **circuit breaker** (resettable mechanical), **resettable/PolySwitch** (solid-state), **inrush current limiter**.
- **Over-voltage / surge:** **MOV/varistor** and **TVS diode** (clamp transients), **gas discharge tube**, **spark gap**, **lightning arrester**.
- **Safety:** **RCD / ground-fault** and **arc-fault** interrupters disconnect on dangerous leakage or arcing.

## Piezoelectric devices, crystals & resonators

Use the **piezoelectric effect** (mechanical stress ↔ voltage):

- **Frequency generation/filtering:** quartz **crystal** (precise clock reference for CPUs, radios), **ceramic resonator** (cheaper, semi-precise), **ceramic** and **SAW filters** (select a frequency band in receivers).
- **Mechanical transducers:** ultrasonic motors, buzzers, sensors.

## MEMS (Microelectromechanical Systems)

Tiny mechanical structures fabricated on silicon — **accelerometers** and gyroscopes (phones, drones), **digital micromirror devices** (DLP projectors), pressure and microphone MEMS. They put mechanical sensing directly onto a chip.

## Motors, actuators & mechanical accessories

- **Actuators:** electric **motors** and **generators**, **solenoids**, and relays convert electrical energy into motion (or back).
- **Mechanical accessories:** enclosures, heat sinks, and fans — the passive mechanical parts that house and cool electronics.

> Obsolete curiosities (coherer, carbon-arc amplifier, dynamo RF generators) are historical only.

Related: [[Passive Components]] · [[Active Components & ICs]] · [[03 Computer Hardware/Motherboard & IO|cooling & connectors in PCs]] · [[02 Electronics|domain overview]]
