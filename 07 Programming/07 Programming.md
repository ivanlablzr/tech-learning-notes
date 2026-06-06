---
type: domain
tags: [domain, moc, programming]
---


Turning intent into instructions the machine executes — and structuring software so humans can maintain it. The goal isn't syntax; it's mastering abstraction layers, system constraints, and the rigor that keeps critical software correct.

## Domain notes

| Note | Covers |
|---|---|
| [[Programming Foundations]] | System/software architecture, P2P, the NASA "Power of 10" rules |
| [[Languages & Applied Programming]] | Core languages, web development, data analytics |

## The five views

**Why it exists / solves:** express computation precisely and reusably above raw machine instructions — abstraction, correctness, maintainability.

**Dependencies:** builds on [[00 Mathematics]] (logic, complexity) and [[04 Operating Systems]] (syscalls, threads, files); depended on by [[08 Databases]], [[10 DevOps]], [[12 Distributed Systems]], [[14 AI]].
```
Math + OS → Programming → (Databases, DevOps, Distributed, AI)
```

**Bridge topic it owns — API contracts/serialization** (REST/gRPC/protobuf: stable, versioned, serializable interfaces between services), plus **concurrency** (reuses the [[04 Operating Systems|OS model]]), **sockets** ([[05 Networking]]), and **message queues** ([[12 Distributed Systems]]).

**Learning path:** fundamentals (types, control flow, functions) → data structures (lists, trees, hash tables, graphs) → algorithms & **Big-O** → paradigms (OO, functional) → Git → testing (unit/integration/e2e) → design patterns → software architecture → APIs.

**Projects:** CLI tool + tests → a REST + gRPC service (same logic, two contracts) → implement a hash map/B-tree from scratch → CI/CD + containers (the L6 anchor in [[Learning Paths & Projects#Project Roadmap]]).

## Mastery roadmap

The most fundamental skill is reading **documentation as the source of truth** — official specs, RFCs, man pages — to solve problems autonomously.

- **Beginner (foundations):** computational thinking and abstraction — variables, control flow, decomposition, simple I/O. Languages: **Python** (English-like) and **C/C++** (via Arduino, to feel the hardware). Projects: console games, basic Arduino.
- **Intermediate (applied):** data structures, OOP, and system/network basics (Linux CLI, Git, HTTP, sockets). Languages: C/C++, Python (automation/parsing), JS/HTML/CSS, Bash. Projects: obstacle-avoiding robot, sensor dashboard, port scanner, chat client.
- **Advanced (system expertise):** deep computer organization ([[04 Operating Systems|OS]], [[Memory]], [[CPU & Processing Units|CPU]]), advanced algorithms/complexity, concurrency, design patterns, [[Cryptography]], threat modeling. Languages: C/C++, **Rust/Go** (safe concurrency), Assembly, Verilog/VHDL. Projects: a Linux driver, security tooling, OS components.

**Language levels:** **compiled / low-level** (Assembly, C, C++, Rust) translate to machine code — more effort to write, faster to run, OS-specific; **interpreted / high-level** (Python, JavaScript, Bash) are human-readable, run line-by-line via an interpreter, portable. **Query languages** (SQL — DDL/DML/DCL) are their own family for data; see [[08 Databases]].

**Specialization branches:** web ([[Languages & Applied Programming]]), network programming (sockets, Scapy, NETCONF/RESTCONF/YANG), security & reverse engineering (exploit dev, ROP, Ghidra/IDA, C2/malware in C/Go/Rust → [[06 Cybersecurity|Ethical Hacking]]), [[10 DevOps]] (IaC, containers, CI/CD), [[12 Distributed Systems]], [[14 AI]], and embedded (below).

## Embedded / microcontroller programming

Programming applied to the physical world. (Hardware, peripherals, and the UART/SPI/I²C protocol details live in [[Digital Logic & Microcontrollers]]; this is the *programming* side.)

- **Platform choice:** an **MCU** (Arduino/ESP32/STM32 — CPU+RAM+Flash+peripherals on one chip, real-time, no OS) vs an **SBC** (Raspberry Pi — runs Linux, not real-time). Use an MCU for hard-real-time, low-power, single-purpose control; an SBC when you need Linux/vision/networking.
- **The Arduino model:** `setup()` runs once, `loop()` forever. Digital/analog I/O via `pinMode`/`digitalWrite`/`analogRead`; PWM via `analogWrite`.
- **Write responsive code:** never block with `delay()` — use **`millis()`-based non-blocking timing** and **state machines**; react to events with short **interrupts (ISRs)** (`volatile` shared vars, minimal work). For true concurrency use **FreeRTOS** (tasks, queues, semaphores — native on ESP32).
- **Mind the memory:** SRAM is tiny (Uno = 2 KB) — store string literals in Flash (`F()`/PROGMEM), avoid dynamic allocation, persist config in EEPROM/NVS.
- **IoT (ESP32):** Wi-Fi + **MQTT** pub/sub, **OTA** firmware updates, and **deep sleep** (~10 µA) for battery life.
- **Firmware security:** flash encryption + secure boot (prevent dumping/tampering), TLS for comms, never hard-code credentials, protect debug headers (JTAG/UART).

Related: [[02 Electronics]] · [[04 Operating Systems]] · [[08 Databases]] · [[14 AI]] · [[Knowledge Graph]] · [[Home]]
