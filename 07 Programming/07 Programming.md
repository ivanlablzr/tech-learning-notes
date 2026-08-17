---
type: domain
tags: [domain, moc, programming]
---


Turning intent into instructions the machine executes — and structuring software so humans can maintain it. The goal isn't syntax; it's mastering abstraction layers, system constraints, and the rigor that keeps critical software correct.

## Domain notes

| Note                                | Covers                                                                                                                |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| [[Programming Foundations]]         | What programming is, how code runs, correct/efficient/secure code, concurrency, debugging, program → system structure |
| [[Data Structures & Algorithms]]    | Big-O, core structures, algorithm families, how to choose                                                             |
| [[Languages & Applied Programming]] | Core languages, web development, data analytics                                                                       |
| [[Software Engineering]]            | SDLC, SOLID & design patterns, testing, code quality, delivery, the software-domain landscape                         |
| [[Programming Learning Resources]]  | How to *get better* — roadmaps, platforms (Exercism, boot.dev), courses, build-your-own, competitive programming, books, project ideas |

## The five views

**Why it exists / solves:** express computation precisely and reusably above raw machine instructions — abstraction, correctness, maintainability.

**Dependencies:** builds on [[00 Mathematics]] (logic, complexity) and [[04 Operating Systems]] (syscalls, threads, files); depended on by [[08 Databases]], [[10 DevOps]], [[12 Distributed Systems]], [[14 AI]].
```
Math + OS → Programming → (Databases, DevOps, Distributed, AI)
```

**Bridge topic it owns — API contracts/serialization** (REST/gRPC/protobuf: stable, versioned, serializable interfaces between services), plus **concurrency** (reuses the [[04 Operating Systems|OS model]]), **sockets** ([[05 Networking]]), and **message queues** ([[12 Distributed Systems]]).

**Learning path:** fundamentals (types, control flow, functions) → data structures (lists, trees, hash tables, graphs) → algorithms & **Big-O** → paradigms (OO, functional) → Git → testing (unit/integration/e2e) → design patterns → software architecture → APIs. (Testing, patterns, SDLC, and delivery are covered in depth in [[Software Engineering]].)

**Projects:** CLI tool + tests → a REST + gRPC service (same logic, two contracts) → implement a hash map/B-tree from scratch → CI/CD + containers (the L6 anchor in [[Master Index — Technology Vault#Project Roadmap]]).

## The domain map

The full territory of programming — every branch, where it's covered, and where the vault has a gap (⚠️). Content lives in the linked notes; this table only maps.

| Branch | Subtopics | Covered in |
|---|---|---|
| **1. Foundations** | variables & types, control flow, functions & scope, recursion, I/O, error handling & exceptions, idioms | [[Programming Foundations]] §1, §4 · [[Languages & Applied Programming]] §1 (Python) |
| **2. Data structures** | arrays/lists, stacks & queues, hash tables, linked lists, trees (BST/B-tree), heaps, graphs, tries — and when to use which | [[Data Structures & Algorithms]] §2, §4 |
| **3. Algorithms** | **Big-O** analysis, sorting & searching, divide-and-conquer, dynamic programming, greedy, graph algorithms (BFS/DFS, Dijkstra) | [[Data Structures & Algorithms]] §1, §3 |
| **4. Paradigms** | imperative/procedural; **OOP** (encapsulation, inheritance, polymorphism, abstraction); **functional** (pure functions, immutability, higher-order, map/filter/reduce); declarative; event-driven | [[Programming Foundations]] §3 |
| **5. Language machinery** | compiled vs interpreted vs **JIT**; type systems (static/dynamic, strong/weak); memory management (manual vs **GC** vs Rust ownership/borrowing); runtimes & VMs | [[Programming Foundations]] §2 |
| **6. Concurrency & parallelism** | threads vs processes vs **async/await**; race conditions; locks/mutexes/semaphores; deadlock; the GIL; channels & actors (Go/Rust) | [[Programming Foundations]] §7 · OS primitives in [[04 Operating Systems]] |
| **7. Working with code** | Git & workflows, **debugging** (breakpoints, stack traces, bisect), **profiling** & optimization, testing, linting/static analysis, documentation | debugging/profiling: [[Programming Foundations]] §8 · rest: [[Software Engineering]] §4–6 |
| **8. Software engineering** | SDLC & agile, SOLID, design patterns, refactoring & tech debt, code review, delivery/CI-CD, secure SDLC | [[Software Engineering]] |
| **9. Architecture** | monolith vs microservices, client-server vs P2P, layering, trustworthy-code discipline | [[Programming Foundations]] §9 → [[13 Architecture]] |
| **10. Interfaces & data formats** | **API contracts** (REST/GraphQL/gRPC, serialization: JSON/protobuf), regex & text processing, file formats (CSV/JSON/YAML/binary), SQL | ⚠️ thin — APIs are this domain's *owned bridge* but have no canonical note; SQL → [[08 Databases]] |
| **11. Applied domains** | web, mobile, desktop, systems, embedded, data, ML/AI, games, security, network programming, HPC | [[Software Engineering]] §8 landscape · [[Languages & Applied Programming]] |

Remaining gap, in priority order: **API contracts** (the owned bridge — feeds [[AI Products & Startup Engineering]] tool-calling and every backend you'll write). Secondary depth targets when needed: regex/text processing, and per-language deep dives (Go/Rust) as they enter use.

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

Related: [[02 Electronics]] · [[04 Operating Systems]] · [[08 Databases]] · [[14 AI]] · [[Master Index — Technology Vault]]
