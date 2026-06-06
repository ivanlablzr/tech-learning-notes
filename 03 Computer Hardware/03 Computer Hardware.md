---
type: domain
tags: [domain, moc]
---


> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

How logic circuits become a machine that executes instructions, stores data, and talks to devices.

> [!question] The five questions
> **Why it exists:** to organize digital logic into a general-purpose, programmable computer.
> **Problem it solves:** turns fixed circuits into something that runs *any* program (the stored-program idea).
> **Depends on:** [[02 Electronics|Electronics]], computer arithmetic from [[00 Mathematics#Overview|Math]].
> **Depended on by:** [[04 Operating Systems|Operating Systems]], [[09 Cloud|Cloud]] (physical servers), [[14 AI|AI]] (GPUs).
> **Interactions:** CPU+memory+I/O+buses form the platform the kernel manages; virtualization later abstracts it away.

### Sub-areas
- **Computer architecture** — von Neumann vs Harvard, the instruction cycle.
- **CPU** — ALU, registers, control unit, pipelines, branch prediction, caches.
- **Memory hierarchy** — registers → L1/L2/L3 cache → DRAM → storage.
- **Storage** — HDD, SSD, NVMe.
- **Interconnects/buses** — PCIe, SATA, USB, DMA, interrupts.
- **Hardware security** — TPM, Secure Boot, hardware root of trust.

See [[Knowledge Graph#Domain Dependency Graph]].

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[03 Computer Hardware#Computer Hardware|Computer Hardware]], [[CPU & Processing Units|Processing Unit]], [[CPU & Processing Units|CPU (Central Processing Unit)]], [[CPU & Processing Units|CPU Cache Memory]], [[CPU & Processing Units|GPU (Graphic Processing Unit)]], [[Memory|Memory Structure]], [[Memory|RAM]], [[Storage#Storage|Storage]], [[Motherboard & IO|Motherboard]], [[Motherboard & IO|Expansion Slots (PCIe, PCI)]], [[Endpoint Security#Endpoint Security|Hardware Security]], [[Hardware Logic & Fabrication|Computational Logic]].

## Learning Path

**Prerequisites:** [[02 Electronics|Electronics]] (sequential logic), computer arithmetic.

1. Computer architecture — stored-program model, fetch-decode-execute.
2. CPU internals — ALU, registers, control unit, the datapath.
3. Machine instructions & assembly — MOV/ADD/JMP; how code becomes execution.
4. Memory hierarchy — caches, locality, virtual vs physical (handoff to OS).
5. I/O — interrupts, DMA, buses (PCIe/USB).
6. Storage technologies — HDD/SSD/NVMe trade-offs.
7. Hardware security — TPM, Secure Boot, root of trust (→ boot/trust bridge).

**Milestones:** trace one instruction through the datapath; explain a cache miss's cost (supports performance reasoning in SRE).

## Dependencies

**Depends on:** [[02 Electronics|Electronics]], [[00 Mathematics#Overview|Math]] (arithmetic).

**Depended on by:**
- [[04 Operating Systems|Operating Systems]] — the kernel manages CPU, memory, devices.
- [[09 Cloud|Cloud]] — physical servers + virtualization.
- [[14 AI|AI]] — GPUs/accelerators.

```text
Electronics → Computer Hardware → Operating Systems → (Networking, Cloud)
```

## Bridge Topics

Canonical owner of two key bridges (full notes in [[Knowledge Graph#Bridge Topic Map]]):

- **Hypervisors / virtualization (Hardware ↔ Cloud)** — *canonical here.* Decouples workloads from physical machines; the basis of [[09 Cloud|cloud]] compute. Types 1 & 2, hardware-assisted virtualization (VT-x/AMD-V).
- **Boot / trust chain (Hardware ↔ OS ↔ Security)** — *canonical here.* UEFI → Secure Boot → TPM → bootloader → kernel; how a dead machine becomes a *trusted* running system.
- **GPU/CUDA (Hardware ↔ AI)** — links to [[14 AI|Bridge Topics]].

## Projects

- **Logic-to-CPU simulator** — extend the L0 project to fetch/decode/execute (see [[Learning Paths & Projects#Project Roadmap]]).
- **Assembly programs** — write small routines; watch registers/memory.
- **Virtualization lab** — run a Type-1 hypervisor, create VMs (bridges into Cloud track).
- **Secure Boot / TPM exploration** — observe measured boot on real hardware.

---
# Reference (consolidated)

## Computer Hardware

> [!info] Knowledge graph
> Part of the systems-thinking map → [[03 Computer Hardware#Overview]] · [[Home]]

#technology #hacking #cybersecurity 

Computation -> Protocols -> Boolean algebra -> **Logical operations** -> Transistors
#### What electronic does it contain?
- Boot Process
	- Power-on → BIOS/UEFI → Bootloader → Kernel → OS
    - MBR vs. GPT
    - POST (Power-On Self Test)
    - GRUB, Secure Boot
- Firmware ([[04 Operating Systems|Operating Systems]])
	- BIOS/UEFI (Unified Extensible Firmware Interface): stored in a flash chip on the motherboard
	- Firmware-level rootkits
	- Embedded systems (routers, IoT, etc)
- TPM (secured on a local device) and HSM (secure data across multiple devices) and Lightweight HSM: chip used for storing cryptographic secret keys and doing encryption
- Bus: data bus, address bus, control bus, system bus, FSB/BSB, I2C/SPI, etc
- [[CPU & Processing Units|Processing Unit]] differ by their programmability or performance as well as which node will use it:
- [[Motherboard & IO|Motherboard]]: the main circuit board that connects all components
- [[Motherboard & IO|Power Supply Unit (PSU)]]: Converts electrical power from an outlet to usable power for the computer
- [[Network Devices|Network Interface Card (NIC)]]: connects the computer to a network
- [[Knowledge Library/Technology/Networking/Network Nodes/Computer Architecture/Storage Devices/Storage|Storage]]: Long-term storage for data and programs
- [[Motherboard & IO|Sound Card]]
- [[Motherboard & IO|Cooling Systems]]
- [[Motherboard & IO|Input-Output Systems]]: allow interaction with the computer
- [[Memory|Memory Structure]]
	- [[Memory|RAM]]
	- [[ROM]]
	- Flash Memory
- Virtualization and Emulation Basics
	- How VMs emulate hardware
	- Hypervisors (Type 1 and 2)
	- VirtualBox, VMware, KVM
- Containerization (Docker, etc)
- TPM (Trusted Platform Module)
- Display Types -> Use cases (gaming, videos and movies, work, just showing information, etc)
	- LCD (Liquid Crystal Display): Light shines through liquid crystals
		- Light source > polarizing filter > electrodes (horizontal) > liquid crystal > electrodes (vertical) > color filter > polarizing filter > display surface
		- Backlight (florescent lamp or LED lights) and inverter (turn DC into AC)
		- Types
			- TN (Twisted Nematic) LCD
				- fast response time (gaming)
				- poor viewing angles - color shifts
			- IPS (In Plane Switching) LCD
				- excellent color
				- more expensive than TN
			- VA (Vertical Alignment) LCD
				- good color
				- slower response times than TN
	- OLED (Organic Light Emitting Diode)
		- Organic compound emits light when receiving an electric current
		- No backlight: the organic compound provides the light
		- Thinner and lighter: flexible and mobile, no glass needed
		- Tablets, phones, smart watches
			- expensive
			- very accurate color
	- Mini LED: same as LCD but with much smaller LEDs (Light Emitting Diodes)
	- Touchscreen
		- digitizers responds to touch 
	- Pixel density: number of pixels per inches -> 27 inch 4K display = 3,840 horizontal divided by 24 inches wide (160 pixels per inches)
	- Refresh rates: number of cycles (images) per second measured in Hz (FPS)
	- Color gamut: the range of colors available on a display
			- sRBG
			- Adobe RGB
			- ITU standards for HDTV
  

#### Who creates these electronic components?


#### How do they create them?

The Creation of microchips is done by a process called [[Hardware Logic & Fabrication|Photolithography]]

#### Resources

Books:
- **“Code: The Hidden Language of Computer Hardware and Software”** by Charles Petzold _(Beginner-friendly)_
- **“Computer Organization and Design”** by David Patterson & John Hennessy _(Intermediate to advanced)_
- **“Practical Reverse Engineering”** by Bruce Dang _(Security-focused)_


Videos & Courses:
- CrashCourse Computer Science (YouTube)
- Computerphile (YouTube)
- Nand2Tetris (free online course on building a computer from scratch)
- MIT OCW: Computer System Architecture
