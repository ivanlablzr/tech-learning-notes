---
type: note
tags: [robotics, mechatronics, prosthetics]
---

> [[18 Robotics & Bio-Convergence]] → building machines that move and act in the physical world, and engineered replacements for the body. Sits at the intersection of mechanical + electrical engineering, materials science, control theory, [[14 AI|AI]], neuroscience and biomedical engineering.

# Robotics & Biomechatronics

Two goals share the same toolbox: **build robots that behave like humans**, and **replace/augment damaged body parts**. The methods overlap (actuators, sensors, control, neural interfaces); the intent differs.

## A robot as a body (the analogy)
| Human | Robot equivalent |
|---|---|
| Bones / joints / tendons | aluminium / carbon-fibre / titanium frames, bearings |
| Muscles | **actuators** — electric motors, hydraulic, pneumatic, artificial muscle |
| Nervous system | CPU → controller → motor driver → actuator |
| Senses (eyes/ears/touch/balance) | cameras, **LiDAR**, microphones, **IMUs**, force/torque sensors |

The control loop — **sense → compute → decide → actuate**, hundreds of corrections/sec — is the robotics core ([[Hardware Interaction & Data Flow|same shape as a hardware control loop]]).

## Humanoid robotics (the hard part)
Walking *looks* trivial but demands real-time **balance control, force control, trajectory planning and feedback** — humans do hundreds of unconscious corrections per second. Bipedal locomotion, manipulation and whole-body control are still unsolved at human level.

## Actuation — artificial muscles
Beyond motors, muscle-like actuators:
- **Shape-memory alloys (SMA)** — metals that contract when heated, relax when cooled.
- **Pneumatic artificial muscles** — e.g. the **McKibben muscle** (compressed air expands/contracts); behaves remarkably like biological muscle.
- **Electroactive polymers (EAP)** — change shape directly under voltage; promising for prosthetics and soft robots.

## Prosthetics — replacing body parts
Current: **myoelectric hands, powered knees, neural-controlled limbs**. The pipeline: `muscle signal → electrodes → computer → artificial hand`.
> **The hard problem isn't movement — it's sensation.** A natural limb has millions of receptors; restoring **artificial touch, temperature, pressure and pain feedback** is the frontier ([[Bio-Convergence Technologies|electronic skin]]).

## Neural interfaces (the future of prosthetics)
Move from `muscle → limb` to `brain → interface → limb` — direct neural control (BrainGate, Neuralink). Full treatment of the bio side in [[Bio-Convergence Technologies]].

## Adjacent fields
- **Tissue engineering** — *grow* replacements instead of building them (stem cells, regenerative medicine, bio-printed organs) → [[Bio-Convergence Technologies]].
- **Biomechanics** — the physics of human movement; humans are astonishingly efficient (a person walks for hours on ~the energy of a small light bulb — most robots can't approach this).
- **Soft robotics** — flexible polymers, air chambers, artificial muscle; widely believed to be the future of humanoids and medical robots.

## The "full human replica" problem
To truly replicate a body you must solve simultaneously: **mechanics** (bones/joints) + **actuation** (muscles) + **sensing** (touch/pressure/temp) + **control** (reflexes/balance) + **intelligence** (perception/planning) + **power**.
> **The battery problem:** a human runs on ~100 W all day and self-repairs; robots need big batteries and a few hours. **Energy storage is one of the biggest bottlenecks in humanoid robotics.**

## Related
[[18 Robotics & Bio-Convergence]] · [[Bio-Convergence Technologies]] · [[02 Electronics]] (actuators, sensors, microcontrollers) · [[14 AI]] (perception, control) · [[03 Computer Hardware]]

#### Resources
*Introduction to Robotics* (Craig) · *Modern Robotics* (Lynch) + free course at modernrobotics.northwestern.edu
