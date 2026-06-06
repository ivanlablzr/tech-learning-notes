---
type: note
tags: [electronics, digital-logic, microcontrollers, embedded]
---


Two linked topics: **digital logic** (how 0s and 1s become arithmetic and memory) and the **microcontroller** (a whole computer — CPU, memory, peripherals — on one chip, built from that logic).

---

# Part A — Digital Logic

Digital circuits represent information as discrete binary states (0/1, LOW/HIGH) rather than continuous analog voltages.

## Number systems & arithmetic

| System | Base | Prefix |
|---|---|---|
| Binary | 2 | `0b` |
| Octal | 8 | `0o` |
| Hex | 16 | `0x` |

- **Two's complement** represents signed integers: invert all bits, add 1 (e.g. −5 in 8-bit = `11111011`). Unsigned 8-bit = 0–255; signed = −128…+127; the **MSB** is the sign bit.
- Binary addition carries (`1+1=10`); subtraction/multiply reduce to add + shift.

## Logic gates

The atoms of digital circuits, built from CMOS transistors. **NAND and NOR are universal** — any function can be made from only one of them (why CMOS favors them).

| Gate | Output is HIGH when… |
|---|---|
| AND `A·B` | all inputs HIGH |
| OR `A+B` | any input HIGH |
| NOT `Ā` | (inverts) |
| NAND / NOR | inverted AND / OR (universal) |
| XOR `A⊕B` | inputs differ |
| XNOR | inputs match |

**Boolean algebra** simplifies logic; the most important identity is **De Morgan's**: `(A·B)' = Ā+B̄` and `(A+B)' = Ā·B̄` — the bridge between AND/OR and NAND/NOR forms.

## Combinational circuits (no memory — output depends only on inputs)

- **Adders** — half adder (`Sum=A⊕B`, `Carry=A·B`); full adder adds a carry-in; chained = **ripple-carry adder** (simple, slow).
- **MUX / DEMUX** — select 1-of-N / route to 1-of-N (a MUX can implement any function as a lookup table).
- **Decoder / encoder** — N bits ↔ one-of-2ᴺ lines (memory addressing, display drivers).
- **Comparator** — outputs A>B, A=B, A<B.

## Sequential circuits (have memory — output depends on inputs + state)

Built from **latches/flip-flops** (full treatment in [[Memory#6. SRAM and the latch (how a bit is physically stored)|Memory]]): SR (foundational), **D flip-flop** (`Q←D` on the clock edge — the workhorse), JK (toggles on `J=K=1`), T (divide-by-2). These compose into:

- **Registers** — N flip-flops storing N bits; **shift registers** (SISO/SIPO/PISO) do serial↔parallel conversion (SPI uses them).
- **Counters** — binary, BCD (0–9), ring, and Johnson counters.

## Memory technologies

| Type | Volatile | Use |
|---|---|---|
| SRAM (6T/bit) | yes | cache — fast, costly |
| DRAM (1T+1C/bit) | yes | main RAM — dense, needs refresh |
| ROM/PROM/EPROM | no | legacy/boot |
| EEPROM | no | config (byte-writable) |
| Flash (NOR/NAND) | no | MCU code / SSDs |

## Analog ↔ digital conversion

- **ADC:** *sample* (Nyquist: > 2× signal frequency) → *quantize* → *encode*. An N-bit ADC has 2ᴺ levels (12-bit = 4096, standard on MCUs). Types: **SAR** (fast, MCU-grade), **sigma-delta** (high-res audio), **flash** (fastest, costly).
- **DAC:** N-bit code → proportional voltage (R-2R ladder; or **PWM + low-pass filter** as a cheap pseudo-DAC).

## CMOS & logic families

Modern logic is **CMOS** — each gate pairs a PMOS pull-up and NMOS pull-down so only one conducts → near-zero static power. **Dynamic power `P = α·C·V²·f`**, so lowering voltage (V²!) is the biggest power lever — why mobile SoCs run low VDD. Watch **logic-level translation** when mixing 5 V and 3.3 V parts.

**Programmable logic:** **CPLD** (small, non-volatile) and **FPGA** (LUTs + flip-flops + routing + DSP/RAM blocks, configured in VHDL/Verilog) give reconfigurable hardware for low-latency/parallel tasks (HFT, SDR, ASIC prototyping).

---

# Part B — Microcontrollers

A microcontroller (MCU) integrates a [[CPU & Processing Units|CPU]] core, **RAM** (data), **Flash/ROM** (program), a **clock**, and **peripherals** (GPIO, timers, ADC/DAC, UART/SPI/I2C, PWM) on a single chip — a complete small computer for embedded control.

## Choosing a class: 8 / 16 / 32-bit

| | 8-bit (AVR/PIC) | 16-bit (MSP430) | 32-bit (ARM Cortex-M) |
|---|---|---|---|
| Clock | ~16 MHz | 16–25 MHz | 48–600 MHz |
| Native int | 8-bit | 16-bit | 32-bit |
| Min sleep | ~0.8 µA | **~0.1 µA** | ~1 µA |
| RAM / Flash | 2–8 KB / 16–256 KB | 0.5–32 KB / 2–256 KB | 4 KB–1 MB / 64 KB–2 MB |
| Best for | cheap, simple control | years on a coin cell | RTOS, DSP, connectivity |

Rule of thumb: simple/cheap → **8-bit AVR (Arduino)**; ultra-low-power sensing → **MSP430**; motor DSP → **dsPIC33**; Wi-Fi/BLE/heavy compute → **32-bit ESP32/STM32**.

## Key chips by class

- **8-bit:** **ATmega328P** (Arduino Uno — 32 KB Flash, RISC, Harvard, 1-cycle instructions), ATtiny85 (8-pin, sub-$1), PIC16/18 (industry staple), legacy 8051.
- **16-bit:** **MSP430** (TI, ultra-low-power, FRAM variants with ~10¹⁵ write endurance), PIC24/dsPIC33 (hardware DSP/MAC for motor control).
- **32-bit (ARM Cortex-M dominates):** cores scale **M0+ → M3 → M4 (DSP+FPU) → M7 (cache, 600 MHz) → M33 (TrustZone security)**. Popular parts: **STM32** ("Blue Pill" F103, F4, H7 flagship), **ESP32** (Wi-Fi+BLE, dual-core; S3 adds AI vector ops; C-series are RISC-V), **RP2040** (Raspberry Pi Pico, unique **PIO** state machines), **nRF52** (best-in-class BLE — AirPods, Fitbit), SAMD21/51.

## How an MCU talks to the world

**GPIO** pins are configurable digital in/out. Practical rules:
- GPIO sources/sinks only ~8–25 mA → drive motors/LED strips through a [[Active Components & ICs#MOSFET — the most-manufactured device on Earth|MOSFET]] or driver IC.
- Floating inputs are undefined → use **pull-up/pull-down** resistors (most MCUs have internal pull-ups).
- Mechanical buttons **bounce** → debounce in software (~20–50 ms) or with an RC filter.
- LEDs need a series resistor: `R = (V_supply − V_f) / I_LED`.

**Serial communication protocols:**

| Protocol | Wires | Clock | Speed | Notes |
|---|---|---|---|---|
| **UART** | 2 (TX/RX) + GND | async (agreed baud) | ~9.6k–115.2k | Point-to-point; debug, GPS, GSM, BT modules. RS-232 needs a MAX232 level shifter |
| **SPI** | 4 (MOSI/MISO/SCK/CS) | sync, master | 1–100+ MHz | Fast, full-duplex; one CS per slave; SD cards, displays, flash |
| **I²C** | 2 (SDA/SCL) + pull-ups | sync | 100 k–3.4 MHz | Multi-device by 7-bit address (≤127); sensors, RTCs, OLEDs |
| **OneWire** | 1 (data+power) | — | low | Dallas DS18B20 temperature sensors |
| **PWM** | 1 | — | — | Duty cycle sets average voltage: LED dimming, motor speed, 50 Hz servos, pseudo-DAC |

**Common peripherals to know:** ADC (read analog sensors via voltage dividers), timers/counters, DMA (move peripheral data to RAM without the CPU — critical for high throughput), and comms modules (HC-05 Bluetooth, ESP Wi-Fi, nRF24L01, LoRa, GSM, NFC).

## 32-bit architecture extras

- **Modified Harvard** bus matrix lets the core fetch code and access data simultaneously; code can run from Flash or RAM.
- **Cache** (M7+) hides slow-Flash wait states; **DMA** offloads bulk transfers (CPU load can drop from 100% to <5%).
- **RTOS** runs comfortably: **FreeRTOS** (tasks, queues, semaphores; ~5–10 KB), Zephyr (rich networking), ThreadX/Azure RTOS. They give preemptive multitasking and deterministic timing for real-time control.

Related: [[Active Components & ICs]] · [[Memory]] · [[CPU & Processing Units]] · [[Programming Foundations]] · [[02 Electronics|domain overview]]
