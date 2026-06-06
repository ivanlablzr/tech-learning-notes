---
type: note
tags: [economics, value-chain, supply-chain]
---

# The Technology Value Chain

Every digital product is the output of a long chain — from rocks in the ground to a recurring software subscription. The single most important economic question is **where in that chain the value (and the margin) actually accumulates**, because it's wildly uneven: a few links capture most of the profit, and those links are also the geopolitical chokepoints.

## 1. The chain — two views

The chain can be read two ways, and you need both.

**(a) The supply flow** — value added in sequence, upstream → downstream:

```
Raw materials → Materials & equipment → Chip design → Fabrication → Packaging
→ Components → Devices/hardware → OS & software → Platforms/apps → Cloud & data → AI/services → End users
```

**(b) The operating system** — in reality it isn't a one-way pipeline but a **feedback system**: capital and data flow *back up*, and AI now optimizes the layers *below* it:

```
┌────────────────────────────────────────────┐
│ 1. HARDWARE & SEMICONDUCTORS               │  chips, memory, interconnect
└───────────────▲──────────────┬─────────────┘
        supplies capacity      │ pulls hardware demand
┌───────────────┴──────────────▼─────────────┐
│ 2. CLOUD INFRASTRUCTURE (utility)          │  aggregates HW into rentable compute
└───────────────▲──────────────┬─────────────┘
        supplies foundation     │ ingests data
┌───────────────┴──────────────▼─────────────┐
│ 3. ENTERPRISE & PLATFORM LOGIC             │  apps/SaaS built on compute
└───────────────▲──────────────┬─────────────┘
        powers marketplaces     │ optimizes experiences
┌───────────────┴──────────────▼─────────────┐
│ 4. CONSUMER EXPOSURE & APPS                │  captures attention, capital, training data
└───────────────▲──────────────┬─────────────┘
   optimizes layers 1–2         │ funnels data + capital back up
┌───────────────┴──────────────▼─────────────┐
│ 5. FRONTIER AI                             │  ingests data from 3 & 4 → optimizes 1 & 2
└────────────────────────────────────────────┘
```

The linear view shows *where value is added*; the layered view shows *why it's a flywheel*. The feedback loops are real and increasingly important:

- **Chips → cloud → AI:** a shortage of AI training chips (1) forces cloud providers (2) to ration compute, which slows and inflates the cost of training frontier models (5).
- **AI → software → chips:** better algorithmic efficiency (5) cuts the compute a SaaS platform (3) needs; AI-assisted design (5) optimizes chip blueprints (1), speeding the fab pipeline.
- **Consumers → cloud → chips:** a shift in consumer demand (4 — e.g. short-form/spatial video) spikes storage and bandwidth (2), forcing infrastructure expansion and bigger memory-chip orders (1).

## 2. The players — who works each link

The same chain, read as **who** operates it (each tier is both a supplier and a customer):

| Tier | Who | Role | Motivation |
|---|---|---|---|
| **Enablers** (upstream) | foundries, mineral refiners, IP firms (**Arm**), research labs | turn raw physical + intellectual capital into components | yield, long-term supply contracts, protect IP |
| **Builders** (integrators) | OEM/ODM, enterprise software, **hyperscalers** (AWS/Azure/GCP) | assemble components into devices, platforms, clouds | economies of scale, time-to-market, supply-chain logistics |
| **Bridges** (distribution) | telcos, **app stores/marketplaces**, VARs, logistics | deliver products + connectivity to users | network effects, low friction, **commission/toll fees** |
| **Demand** (downstream) | consumers, enterprises, governments | buy/subscribe/license — the destination that **injects capital back** | utility, cost/performance, integration |
| **Services** (last mile) | consultancies, MSPs, **independent consultants** | help clients adopt, secure, and run the tech correctly | (the layer **you** are building toward — see [[Master Roadmap]]) |

## 3. Where value is captured — the "smile curve"

Plot value-added against the chain and you get a **smile**: high at the two ends (**upstream IP/design + equipment**, and **downstream brand/software/services**), low in the middle (**manufacturing and assembly**).

| Stage | Who dominates | Economic character |
|---|---|---|
| **Raw materials / refining** | China (~70% of processing), DRC (cobalt), Taiwan/Japan (chemicals) | extraction + chemistry; **low margin but high leverage** (chokepoint) |
| **Semiconductor equipment** | **ASML** (EUV monopoly), Applied Materials, Lam, Tokyo Electron | oligopoly; very high margin; R&D-intensive |
| **Chip design (fabless)** | **Nvidia**, AMD, Apple, Qualcomm, Arm (IP) | **highest margin**; pure IP + people, no factories |
| **Fabrication (foundry)** | **TSMC** (~60–70% of leading-edge), Samsung, Intel | most **capital-intensive** business on earth; scale economics |
| **Packaging / assembly** | TSMC (CoWoS), ASE, Amkor; assembly in China/Vietnam | rising importance (advanced packaging); thin assembly margins |
| **Devices / OEM** | Apple, Samsung, Dell, the China assembly base | brand captures value; **assembly is razor-thin** |
| **OS & platforms** | Microsoft, Apple, Google, Meta, Amazon | **network effects → near-monopoly rents** |
| **Cloud / data / AI** | AWS, Azure, GCP; OpenAI/Anthropic | capital-intensive but **recurring, high-retention** revenue |

- The **fabless** model (design without fabs) keeps the high-margin IP and offloads the capital-heavy middle to foundries — Nvidia's gross margins run **~70%+**; a contract assembler's run in the **low single digits**.
- The classic illustration: of an iPhone's retail price, **factory assembly** in China historically captured only a few dollars, while **Apple's design/brand/software** and **US component IP** captured the bulk. "Made in China" ≠ "value captured by China."
- So **owning the IP and the customer relationship beats owning the factory** — and nations that only do assembly stay poor in the chain even with huge export volumes.

## 4. Why tech economics are different

- **Near-zero marginal cost** — software/digital goods cost ~nothing to copy; once R&D is paid, each extra unit is almost pure margin → **scale is everything**.
- **High fixed / sunk cost** — a leading-edge **fab costs ~$20–40B**; a frontier AI model costs hundreds of millions to train. Huge up-front, tiny per-unit → favors giants.
- **Intangible capital** — value sits in **R&D, IP, brand, data, software**, not factories. Modern tech balance sheets are "capital-light" — except the new exception, AI compute (see [[Cloud, Data & AI Economics]]).
- **Network effects & winner-take-most** — value rises with users, tipping markets toward one or two winners (detail in [[The Platform & Digital Economy]]).
- **Increasing returns** — unlike traditional goods (diminishing returns), digital leaders get *cheaper and better* as they scale, entrenching dominance.

## 5. What shapes the chain — macro & micro forces

The players don't act in a vacuum; structural forces continually reshape the chain:

- **Macro (the environment):** **geopolitics & tech nationalism** (export controls, sanctions, minerals access — [[US-China Tech War & Export Controls]]); **capital & interest rates** (deep tech is capex-heavy — cheap money fuels speculative R&D, high rates force a pivot to short-term profit); **regulation** (antitrust, data-sovereignty rules like GDPR/CCPA, AI-safety frameworks reshape cost structures).
- **Micro (the market forces):** **switching costs & lock-in** (sticky revenue, but it blocks disruptive newcomers); the **talent war** (AI researchers, silicon architects — scarcity becomes a product bottleneck); **compute & energy constraints** (affordable power + advanced chips are the physical ceiling on AI scaling — see [[Cloud, Data & AI Economics]]).

## 6. Chokepoints — the few links that hold everyone hostage

The chain has **single points of control** that confer outsized economic and political power:

| Chokepoint | Controller | Why it can't be quickly replaced |
|---|---|---|
| **EUV lithography** | ASML (Netherlands) | the only maker of the machines for sub-7nm chips; decades of accumulated know-how |
| **Leading-edge fabrication** | TSMC (Taiwan) | concentrated process expertise; new fabs take 3–5 yrs + tens of $B |
| **AI accelerators** | Nvidia + **CUDA** software lock-in | hardware *and* the developer-ecosystem moat |
| **Chip IP** | Arm (instruction sets), Synopsys/Cadence (EDA tools) | every chip is designed with a handful of tools/IP |
| **Mineral refining** | China (~70%; gallium ~99%, rare earths ~91%) | refining capacity + environmental tolerance, not just deposits |

Control a chokepoint and you can throttle a rival's entire industry — the heart of [[US-China Tech War & Export Controls|the chip war]].

## 7. Reading the whole chain as an economy

The punchline: **a handful of firms at the high-margin ends of the chain capture the lion's share of a >$20-trillion digital economy (~17% of world GDP)**, while the capital-heavy middle and the resource base operate on thin margins but enormous strategic leverage. The rest of this domain zooms into the links that matter most — [[Semiconductor Economics|silicon]], [[The Platform & Digital Economy|platforms]], and [[Cloud, Data & AI Economics|cloud/AI]] — then asks who controls them ([[Tech Sovereignty & Governance]]).

Related: [[17 Tech Economy & Geopolitics|domain overview]] · [[Semiconductor Economics]] · [[The Platform & Digital Economy]] · [[Cloud, Data & AI Economics]] · [[03 Computer Hardware]]
