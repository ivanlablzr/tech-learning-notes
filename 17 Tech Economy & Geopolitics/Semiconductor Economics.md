---
type: note
tags: [economics, semiconductors, supply-chain]
---

# Semiconductor Economics

Chips are the **base of the entire tech economy** — and the industry that makes them is the strangest in capitalism: trillion-dollar demand resting on a handful of firms, each a near-monopoly at its layer, running on the most brutal capital intensity anywhere. (The technology is in [[03 Computer Hardware]]; this note is the *business* of silicon.)

## 1. The market

Global semiconductor sales hit **~$792B in 2025 (+25.6%)** and are on track to cross **$1 trillion** — driven overwhelmingly by **AI**: logic (~$302B) and memory (~$223B) are the biggest categories, and **Nvidia alone went from ~$130B revenue (FY25, +114%) to a ~$57B *quarter* (Q3 FY26)**. The chip industry is now both the **physical foundation** and the **financial engine** of the AI boom.

## 2. The three business models

| Model | What it does | Example | Economics |
|---|---|---|---|
| **IDM** (integrated) | designs *and* fabricates | Intel, Samsung, Micron | owns everything; huge capex; vulnerable if its fab falls behind |
| **Fabless** | designs only, outsources fab | **Nvidia**, AMD, Apple, Qualcomm | **highest margin**, capital-light, IP-driven |
| **Foundry** (pure-play) | fabricates others' designs | **TSMC**, GlobalFoundries | capital-heavy; wins on scale + yield |

The industry **disaggregated** over decades: design (fabless) split from manufacturing (foundry) because each got too expensive to do both well. TSMC's founding insight (1987) — *be the neutral foundry for everyone* — created the model that now concentrates the world's leading-edge production in Taiwan. Layer in **IP/EDA** (Arm, Synopsys, Cadence — every chip starts here) and **equipment** (ASML, Applied Materials, Lam, Tokyo Electron) and you have the full stack.

## 3. Capital intensity — why only a few can play

This is the defining economic fact:

- A **leading-edge fab costs ~$20–40B** and is obsolete in a few years; an **EUV machine costs ~$200M+** (and a next-gen High-NA unit ~$380M).
- Fabs must run near 100% utilization to pay back; a downturn is financially catastrophic → the famous **boom-bust cycle**.
- R&D + capex eat a huge share of revenue, so **scale is survival**: each node shrink costs more, so fewer firms can afford the next one. The leading edge has collapsed from ~20+ firms in 2000 to effectively **three** (TSMC, Samsung, Intel) — and really **one** at the frontier.

## 4. Moore's Law as economics

**Moore's Law** (transistor count doubling ~every 2 years) was always an *economic* observation, not just physics: it meant **cost-per-transistor fell relentlessly**, which is what made computing pervasive. That economic engine is now sputtering:

- Physical limits + skyrocketing fab/design costs mean **cost-per-transistor has stopped falling** at the leading edge.
- The industry compensates with **advanced packaging** (chiplets, TSMC's CoWoS, 3D stacking) — assembling multiple smaller dies — and **specialization** (GPUs, TPUs, NPUs) instead of pure shrinks. Packaging capacity (CoWoS) is itself now a bottleneck for AI chips.

## 5. The chokepoints (where the leverage lives)

| Layer | Controller | Moat |
|---|---|---|
| **EUV lithography** | **ASML** (NL) — sole maker | the hardest machine humanity builds; projecting ~$71B revenue by 2030 |
| **Leading-edge foundry** | **TSMC** (~60–70% of leading-edge; foundry market ~$320B in 2025) | process know-how + yield; customers (Apple, Nvidia, AMD) depend on it |
| **AI accelerators** | **Nvidia** | GPU performance **+ CUDA** software lock-in (the real moat) |
| **EDA tools** | Synopsys, Cadence (+ Siemens) | you literally cannot design a chip without them |
| **Chip IP** | **Arm** | instruction-set architecture in ~every phone + increasingly servers |

Concentration this extreme is why semiconductors are the **central front of [[US-China Tech War & Export Controls|tech geopolitics]]** — controlling ASML or TSMC means controlling whether a rival can make advanced chips at all.

## 6. The Taiwan question — the "Silicon Shield"

The world's most advanced chips are made on an island China claims and the US is pledged to help defend. **TSMC in Taiwan is a single point of failure for the global economy**: a conflict or blockade would halt advanced-chip supply to everyone, an event often estimated in the **trillions** of lost output. This concentration drives the entire **re-shoring** wave — the US CHIPS Act, TSMC's Arizona/Japan/Germany fabs, the EU Chips Act — all attempts to buy geographic insurance against a Taiwan shock (see [[Tech Sovereignty & Governance]]). The "Silicon Shield" theory holds that Taiwan's indispensability is itself a deterrent.

## 7. The cycle & the risk

Semiconductors are violently **cyclical** (capacity is lumpy and lags demand by years). The current AI super-cycle has pushed leading-edge demand and Nvidia's margins to records — but the same dynamics that built it (huge capex, [[Cloud, Data & AI Economics#5. Is it a bubble?|circular AI financing]]) make a correction consequential: an AI-demand stumble would leave very expensive fabs and inventory underwater.

Related: [[17 Tech Economy & Geopolitics|domain overview]] · [[The Technology Value Chain]] · [[Cloud, Data & AI Economics]] · [[US-China Tech War & Export Controls]] · [[03 Computer Hardware]] · [[East-Asia Digital Infrastructure|Taiwan]]
