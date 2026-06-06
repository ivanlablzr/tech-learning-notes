---
type: note
tags: [hardware, motherboard, io]
---


The motherboard is the central PCB that physically mounts and electrically connects every component — [[CPU & Processing Units|CPU]], [[Memory|RAM]], [[Storage]], [[CPU & Processing Units#3.1 GPU (Graphics Processing Unit)|GPU]], peripherals — plus the firmware that brings the machine to life, the power that feeds it, and the cooling that keeps it alive.

## 1. The board

**Form factor** sets size, slot count, and case/PSU compatibility:

| Form factor | Size | PCIe slots | Use |
|---|---|---|---|
| E-ATX | 305×330 | 5–7 | Workstation/HEDT |
| ATX | 305×244 | 4–7 | Standard desktop |
| mATX | 244×244 | 2–4 | Compact desktop |
| Mini-ITX | 170×170 | 1 | Small form factor |

**CPU socket** is platform-specific (not interchangeable): Intel **LGA** (pins in socket) e.g. LGA 1700/1851; AMD moved to LGA with **AM5** (after PGA on AM4); server sockets are huge (EPYC SP5 ≈ 6096 pins).

**Chipset:** modern CPUs absorbed the memory and PCIe controllers onto the die (Intel Sandy Bridge 2011, AMD Zen 2017), so the old **northbridge** vanished. What remains is the **PCH (Platform Controller Hub)** handling SATA/USB/audio/LAN and extra PCIe lanes, linked to the CPU over **DMI** (~8–16 GB/s).

**VRM (Voltage Regulator Module):** steps the PSU's 12 V down to the CPU's ~0.9–1.4 V using PWM controller + MOSFETs + chokes + caps. More **phases** = smoother power and better sustained boost; weak VRMs throttle high-TDP chips.

## 2. Buses, slots & I/O

**PCIe** is the universal expansion interconnect; bandwidth per lane doubles each generation (3.0 ≈ 1 GB/s/lane, 4.0 ≈ 2, 5.0 ≈ 4). A physical ×16 slot may run electrically at ×16/×8/×4.

| Slot | Typical use |
|---|---|
| PCIe ×16 | GPU, high-bandwidth cards |
| PCIe ×4/×1 | NVMe cards, NICs, sound, controllers |
| **M.2** | NVMe/SATA SSDs, Wi-Fi (M vs B+M keying) |

**I/O & the CPU talk via:** the system bus, **interrupts** (devices signal the CPU), **DMA** (devices move data to/from RAM without the CPU), and device drivers. *Security relevance:* malware hides in drivers; **DMA attacks** (e.g. over Thunderbolt/PCIe) and cold-boot attacks read memory directly, bypassing the OS.

## 3. Firmware & boot (BIOS → UEFI)

**UEFI** replaced legacy 16-bit BIOS: 64-bit, GPT/>2.2 TB disks, GUI, PXE, and **Secure Boot** (verifies the bootloader's cryptographic signature to block bootkits/rootkits — part of the [[CPU & Processing Units|hardware]]→OS→security boot/trust chain).

**POST (Power-On Self-Test)** on power-up: CPU reset vector → chipset init → memory training/test → device enumeration → boot-device select → hand off to the bootloader (GRUB / Windows Boot Manager). Failures surface as beep codes or debug LEDs (no display usually = RAM or CPU).

**CMOS battery** (CR2032) keeps the real-time clock and firmware settings (boot order, XMP) while unplugged; clearing it resets a board to defaults — the standard fix for a bad-overclock no-boot.

## 4. Power (PSU)

Converts wall **AC → DC** via transformer → rectifier → filters/regulators, delivering the rails components need:

| Rail | Feeds |
|---|---|
| +12 V | GPU/PCIe, drive motors, fans (most power) |
| +5 V / +3.3 V | Logic, RAM, M.2 |
| +5 VSB | Standby (power button, Wake-on-LAN) |

`Watts = Volts × Amps`. Pick wattage to cover all components (online calculators help). **80 PLUS** ratings (Bronze→Titanium) measure conversion efficiency — higher = less wasted power and heat. **Modular** PSUs let you attach only the cables you need.

## 5. Cooling & audio

- **Cooling:** fans + heatsinks (air) or liquid loops move heat away to prevent thermal throttling/damage; cooler choice tracks the CPU's [[CPU & Processing Units#1.4 Performance characteristics|TDP]].
- **Sound card:** DAC (digital→analog out) and ADC (analog→digital in) for audio I/O; mostly integrated into the chipset now.

Related: [[CPU & Processing Units]] · [[Memory]] · [[Storage]] · [[03 Computer Hardware|domain overview]]
