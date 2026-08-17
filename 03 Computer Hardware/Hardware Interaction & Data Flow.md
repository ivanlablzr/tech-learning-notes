---
type: note
tags: [hardware, architecture, buses, datapath]
---

How the components of [[03 Computer Hardware|a computer]] actually *talk to each other*. Individually they're inert; what makes a "computer" is the **orchestrated flow of data and control** between [[CPU & Processing Units|CPU]], [[Memory|memory]], [[Storage]], and I/O over a system of **buses**. This is the *inside-one-machine* counterpart to the [[OSI Layers & Protocols|OSI stack]] (which is about communication *between* machines).

> **One-line model:** the CPU is the only part that *thinks*; everything else either *stores* (memory/storage) or *talks to the outside* (I/O). Buses are the roads; the CPU is constantly fetching from memory, computing, and writing back.

## 1. Buses — the roads between components
A **bus** is a shared set of wires carrying data between components. Classic system bus = three logical channels:

| Bus | Carries | Direction | Example question it answers |
|---|---|---|---|
| **Address bus** | *which* location to access | CPU → memory/IO (one-way) | "Read from address 0x1A40" |
| **Data bus** | the actual bits | two-way | "Here are the 64 bits" |
| **Control bus** | *what* to do + timing | two-way | "This is a READ, now" |

- **Width matters:** address-bus width sets how much memory is addressable (32-bit → 4 GB; 64-bit → effectively unlimited). Data-bus width sets how many bits move per cycle.
- **Modern interconnects** (on the [[Motherboard & IO|motherboard]]): **PCIe** (GPU, NVMe — serial, lane-based), **SATA** (drives), **USB** (peripherals), **DDR bus** (to RAM), **I²C/SPI** (low-speed sensors/firmware). The old single shared bus has become a hierarchy of point-to-point links for speed.

## 2. The datapath — fetch → decode → execute
Everything the machine does is the CPU repeating the **instruction cycle** ([[CPU & Processing Units|CPU internals]]):

1. **Fetch** — the **Program Counter (PC)** holds the address of the next instruction; the CPU puts it on the address bus and reads the instruction from [[Memory|RAM]] into the **Instruction Register**.
2. **Decode** — the **Control Unit** interprets the opcode (what operation, which registers/memory).
3. **Execute** — the **ALU** does the math/logic; results go to **registers**.
4. **Write-back** — store the result to a register or back to memory; advance the PC.

This loop runs billions of times per second. **Pipelining** overlaps the stages (fetch instruction N+1 while decoding N), and **branch prediction** guesses the path through `if`-statements to keep the pipeline full.

## 3. The memory hierarchy — why data sits where it does
The CPU is far faster than memory, so data is staged across levels, fast/small → slow/large ([[Memory]], [[Storage]]):

```
Registers  →  L1/L2/L3 cache (SRAM)  →  Main memory (DRAM)  →  Storage (SSD/HDD)
 <1ns           ~1–20ns                  ~100ns               ~100µs–10ms
```

- **Locality** is why caches work: programs reuse the same data (temporal) and nearby data (spatial). A **cache hit** is ~100× faster than going to DRAM; a **cache miss** stalls the CPU for ~hundreds of cycles (this is why a "cache miss" is the cost that performance/[[11 SRE|SRE]] reasoning cares about).
- **Virtual memory:** the [[04 Operating Systems|OS]] gives each process its own address space; the CPU's **MMU** translates virtual→physical addresses (the hardware↔OS handoff).

## 4. Talking to the outside — interrupts, polling, DMA
How the CPU deals with slow I/O devices (keyboard, disk, NIC) without wasting cycles:

- **Polling** — CPU repeatedly checks "ready yet?" Simple but wasteful.
- **Interrupts** — the device *signals* the CPU when ready; the CPU drops what it's doing, runs an **Interrupt Service Routine**, then resumes. Efficient; this is how a keypress or an arriving network [[OSI Layers & Protocols|packet]] gets attention.
- **DMA (Direct Memory Access)** — for bulk transfers (disk→RAM, [[Network Devices|NIC]]→RAM), a **DMA controller** moves data *directly* between device and memory, interrupting the CPU only when the whole block is done. Frees the CPU entirely during the transfer.

## 5. The boot / trust chain — dead silicon → running system
How a powered-off machine becomes a *trusted* OS (the Hardware↔[[04 Operating Systems|OS]]↔[[06 Cybersecurity|Security]] bridge):

**Power on → POST** (self-test) **→ firmware (UEFI/BIOS** on a flash chip) **→ bootloader (GRUB) → kernel → OS.**
Security layers ride along: **Secure Boot** checks each stage's signature; the **TPM** stores keys and *measures* the boot (hardware root of trust). A firmware-level rootkit subverts this whole chain from below — which is why hardware trust matters.

## 6. Putting it together — trace a single keypress
1. You press a key → keyboard controller raises an **interrupt** (§4).
2. CPU suspends its current instruction, jumps to the keyboard **ISR** (§2 control flow).
3. ISR reads the scan-code over the bus (§1) into a register, hands it to the [[04 Operating Systems|OS]] driver.
4. OS places the character in the focused app's input buffer in [[Memory|RAM]] (§3).
5. App logic runs (more fetch-decode-execute, §2); to display it, the result is written to a framebuffer the [[CPU & Processing Units|GPU]] scans out to the screen.
Every layer above — OS, app, network — ultimately reduces to *this*: data moving over buses between CPU, memory and I/O.

## Where this sits
- **Below:** [[02 Electronics|electronics]] and [[Hardware Logic & Fabrication|logic gates]] build these components.
- **Beside:** the [[OSI Layers & Protocols|OSI/TCP-IP stack]] handles data *between* machines; *this* note handles data *inside* one.
- **Above:** the [[04 Operating Systems|OS]] abstracts all of this so [[07 Programming|programs]] don't manage buses and interrupts by hand.
- **The big picture:** see [[Master Index — Technology Vault]].

## Related
[[CPU & Processing Units]] · [[Memory]] · [[Storage]] · [[Motherboard & IO]] · [[Hardware Logic & Fabrication]] · [[03 Computer Hardware]] · [[04 Operating Systems]]
