---
type: note
tags: [hardware, processors]
---


How a computer turns instructions into work. This note covers the general-purpose **CPU** first (the foundation), then the specialized processors that offload specific workloads — **GPU**, **DPU**, and **NPU** — and closes with the wider processor family.

> **Mental model:** a CPU is a race car (a few cores, blazing fast at one task at a time); a GPU is a bus (thousands of slow seats, enormous throughput); a DPU is the logistics fleet (moves and transforms data so the CPU doesn't have to); an NPU is the assembly line inside routers (forwards packets at wire speed).

---

## 1. The CPU (Central Processing Unit)

The CPU executes program instructions. Everything else on this page exists to take work *off* it.

### 1.1 Core building blocks

| Unit | Role |
|---|---|
| **Control Unit (CU)** | Fetches and decodes instructions, directs the other units |
| **Arithmetic Logic Unit (ALU)** | Performs the actual arithmetic and boolean logic |
| **Registers** | Tiny, fastest-possible storage *inside* the CPU — e.g. Program Counter (PC), Current Instruction Register (CIR), Accumulator (AC), Memory Address/Data Registers (MAR/MDR) |
| **Cache** | Small on-die SRAM that holds recently used data/instructions (see §2) |

### 1.2 The instruction cycle

Every instruction flows through the same loop, measured in clock cycles (Hz):

1. **Fetch** — read the next instruction from cache/[[Memory|RAM]]
2. **Decode** — the CU translates it into control signals
3. **Execute** — the ALU performs the operation
4. **Write-back** — the result is stored to a register or memory

### 1.3 Buses

The CPU talks to memory and I/O over three buses: the **address bus** (which location), the **data bus** (the actual bytes), and the **control bus** (read/write timing signals). High-speed point-to-point interconnects — PCIe, Intel QuickPath/UPI, AMD Infinity Fabric, HyperTransport — have largely replaced shared parallel buses.

### 1.4 Performance characteristics

| Metric | What it measures |
|---|---|
| **Cores** | Independent execution units; more cores = more parallel tasks |
| **Threads (SMT/Hyper-Threading)** | Logical cores letting one physical core interleave two instruction streams |
| **Clock speed (GHz)** | Billions of cycles per second |
| **IPC** | Instructions completed per cycle — a function of microarchitecture, not clock |
| **TDP (W)** | Heat output under load; dictates the cooler you need |
| **Word size** | A 64-bit CPU addresses/operates on 64-bit values (up to 2⁶⁴) |

> Real performance ≈ cores × clock × IPC. A higher-IPC chip can beat a higher-clocked one.

### 1.5 Instruction Set Architecture (ISA)

The ISA is the contract between hardware and software — the registers, instruction types (arithmetic, logical, control-flow, memory), and addressing modes (immediate, direct, indirect, indexed) the CPU exposes.

- **CISC (x86/x64)** — many complex, variable-length instructions; decades of backward compatibility.
- **RISC (ARM, RISC-V)** — a small, uniform instruction set; simpler decoding means more performance per watt, which is why ARM dominates mobile and is now competitive in servers and laptops.

### 1.6 Advanced microarchitecture

Techniques that raise IPC by doing more per cycle:

- **Pipelining** — overlap fetch/decode/execute of successive instructions.
- **Superscalar** — multiple execution units issue several instructions per cycle.
- **Out-of-order execution** — run ready instructions while others wait on data.
- **Branch prediction & speculation** — guess the path of a branch to keep the pipeline full.
- **Simultaneous multithreading (SMT)** — fill idle execution slots with a second thread.
- **Hazards** (structural, data, control) are the stalls these features work around.

> *Security relevance:* speculative execution is the root of the Spectre/Meltdown class of side-channel attacks — exploit and reverse-engineering work happens directly at the instruction/register level.

---

## 2. Cache & the memory hierarchy

Cache hides the latency gap between the fast CPU and slow [[Memory|RAM]].

| Level | Scope | Speed |
|---|---|---|
| **L1** | Per core | Fastest; runs at core speed |
| **L2** | Per core | Catches what L1 misses |
| **L3** | Shared across all cores | Catches what L2 misses |

- Cache is built from **[[Memory|SRAM]]** — fast, no refresh needed, but expensive and low-density. Main memory uses **[[Memory|DRAM]]**, which is denser/cheaper but must be constantly refreshed.
- The **Memory Management Unit (MMU)** translates virtual addresses to physical ones and enforces paging/segmentation — the hardware basis of process isolation and memory virtualization.

---

## 3. Specialized processors

Modern systems offload specific work to purpose-built silicon so CPU cores stay free for application logic.

### 3.1 GPU (Graphics Processing Unit)

Originally for rendering, now the workhorse of AI/ML, simulation, and password cracking. Where a CPU minimizes **latency** with a few complex cores, a GPU maximizes **throughput** with thousands of simple ones.

| | CPU | GPU |
|---|---|---|
| Cores | 4–64 complex | 1,000s–10,000s simple |
| Optimized for | Latency, serial code | Throughput, data-parallel |
| Memory BW | ~50–100 GB/s | 0.5–3.35 TB/s (GDDR6X/HBM) |
| Weakness | Limited parallelism | Branch divergence serializes threads |

- **Execution model — SIMT:** threads run in lock-step groups (NVIDIA **warp** = 32, AMD **wavefront** = 64), all executing one instruction on different data. Ideal for matrix math and image filtering; *warp divergence* (threads taking different branches) hurts efficiency.
- **Memory hierarchy:** registers → shared memory/L1 (per SM) → L2 → VRAM → host RAM over PCIe. The **SM (Streaming Multiprocessor)** is the basic compute cluster.
- **Tensor Cores** do a 4×4 fused multiply-add (`D = A×B + C`) per cycle in mixed precision (FP8/FP16/BF16/TF32), which is exactly what neural-network training needs.
- **Programming:** **CUDA** (NVIDIA-only, dominant in AI thanks to cuBLAS/cuDNN), **OpenCL** (portable), and compute shaders (Vulkan/DirectX).

| Data-center/consumer GPU | Cores | Memory | Notable |
|---|---|---|---|
| A100 (Ampere) | 6,912 CUDA | 80 GB HBM2e, 2 TB/s | 312 TFLOPS FP16 |
| H100 (Hopper) | 16,896 CUDA | 80 GB HBM3, 3.35 TB/s | FP8 + Transformer Engine |
| RTX 4090 (Ada) | 16,384 CUDA | 24 GB GDDR6X | 82.6 TFLOPS FP32 |

> *Security relevance:* GPUs crack fast hashes (MD5/NTLM/SHA-1) at hundreds of GH/s — an 8-char lowercase password falls in seconds. Memory-hard, deliberately slow hashes (**bcrypt, scrypt, Argon2**) blunt this by defeating GPU parallelism.

### 3.2 DPU (Data Processing Unit / SmartNIC)

A programmable processor that offloads **infrastructure** work — networking, storage, security — from the host CPU. Typically ARM/RISC-V cores + hardware accelerators + 25–400 GbE ports on a PCIe card.

- **Networking:** packet processing, VXLAN/GENEVE overlays, Open vSwitch offload, SR-IOV.
- **Security:** line-rate TLS/IPsec, stateful firewall/ACLs, deep packet inspection, micro-segmentation enforced at the NIC.
- **Storage:** NVMe-over-Fabrics, compression/dedup, erasure coding.
- **Isolation:** cloud providers run their hypervisor, virtual networking, and billing on the DPU, fully separated from the tenant OS (the model behind AWS Nitro).

Without a DPU, 100 Gbps of traffic can eat 30–40% of a server's cores. Key products: **NVIDIA BlueField**, **Intel IPU**, **AMD Pensando**, **Marvell OCTEON**.

### 3.3 NPU (Network Processing Unit)

> "NPU" is overloaded: here it means the **networking** packet-forwarding engine (not the AI "Neural Processing Unit" in §3.4).

A programmable ASIC that forwards packets at **line rate** with deterministic, sub-microsecond latency — the data-plane heart of high-end [[Switching & Routing|routers]] and [[Switching & Routing|switches]]. At 400 GbE a packet arrives every ~1.3 ns, far too fast for a CPU.

- **Pipeline:** Parser → Classifier → Lookup → Modifier → Scheduler → Output, every stage in parallel hardware.
- **Memories:** **TCAM** (wildcard matching for ACLs/routes), **SRAM** (tables/counters), **HBM** (millions of routes).
- **Operations:** longest-prefix-match routing, MAC lookup, MPLS, ACLs, NAT/PAT, QoS marking/policing, VXLAN/GRE tunneling, in-band telemetry.
- **Control vs data plane:** routing protocols (BGP/OSPF) run on the device **CPU**; per-packet forwarding runs on the **NPU**. Vendors: Cisco Silicon One, Broadcom Trident/Tomahawk, Marvell, Intel Tofino (P4-programmable), Juniper Express/Trio.

### 3.4 The wider processor family

| Processor | Specialty |
|---|---|
| **NPU (Neural)** | On-device ML inference (mobile SoCs, AI accelerators) |
| **TPU** | Google's tensor accelerator for training/inference |
| **DSP** | Real-time signal processing (audio, radio, sensors) |
| **FPGA** | Reconfigurable logic — custom pipelines, low-latency finance/networking |
| **Crypto processor** | Hardware encryption/key handling (HSM, secure enclave) |
| **QPU** | Quantum processing — see [[Quantum Computing & Technology]] |

---

## How it fits together

Physics → [[Digital Logic & Microcontrollers|logic gates]] → ALU/registers → CPU. The CPU then delegates: parallel math to the **GPU**, infrastructure I/O to the **DPU**, packet forwarding to the **NPU**. All of them depend on the [[#2. Cache & the memory hierarchy|memory hierarchy]] and connect over the [[Motherboard & IO|motherboard]]'s buses.

Related: [[Memory]] · [[Storage]] · [[Hardware Logic & Fabrication]] · [[Motherboard & IO]] · [[03 Computer Hardware|domain overview]]
