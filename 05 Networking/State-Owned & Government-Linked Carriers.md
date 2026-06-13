---
type: note
tags: [networking, infrastructure, governance, carriers]
---


A carrier is "state-owned" when a government holds a controlling or significant stake — directly or via a sovereign wealth fund / state holding company. This matters because it shapes **pricing, censorship capability, lawful-intercept compliance, and geopolitical leverage** over international routing. (Context: [[Network Infrastructure]], [[Submarine Cables]].)

## Fully state-owned (≈100%)

| Carrier | Country | ASN | Note |
|---|---|---|---|
| **China Telecom / Unicom / Mobile** | China | AS4134/4837/9808 | the "Big Three" under MIIT/SASAC; control all gateways |
| **BSNL / MTNL** | India | AS9829/17813 | Dept. of Telecom, 100% |
| **Ethio Telecom** | Ethiopia | AS24757 | monopoly until 2021 |
| **VNPT / Viettel** | Vietnam | AS7643/38731 | Viettel owned by **Ministry of Defence** |
| **TCI** | Iran | AS12880 | Islamic Republic |
| **UzTelecom / Turkmen / Tajik / Kyrgyz** | Central Asia | — | state monopolies |
| **ETECSA** | Cuba | AS27725 | sole operator, monopoly since 1994 |
| **KPTC / Star JV** | North Korea | AS131279 | ~30 routable IPs for the whole country |
| **du (EITC)** | UAE | AS15802 | Dubai govt |

Also fully/near-fully state: Nepal Telecom, Bhutan Telecom, Afghan Telecom, Iraq Telecom, Algeria Telecom, Tunisie Telecom (65%), and several Pacific/island operators.

## Majority or significant state stakes

| Carrier | Country | Govt stake | Held via |
|---|---|---|---|
| **Swisscom** | Switzerland | ~51% | Confederation |
| **Telenor** | Norway | ~54% | Ministry of Trade |
| **Proximus** | Belgium | ~53% | federal state |
| **SingTel** | Singapore | ~52% | **Temasek** |
| **Telkom Indonesia** | Indonesia | ~52% | Ministry of SOEs |
| **KazakhTelecom** | Kazakhstan | ~52% | Samruk-Kazyna |
| **Telia** | Sweden | ~37% | state + pensions |
| **Rostelecom** | Russia | ~38% | Rosimushchestvo |
| **Deutsche Telekom** | Germany | ~32% | KfW bank |
| **NTT** | Japan | ~33% | Ministry of Finance |
| **Telecom Egypt** | Egypt | ~80% | Egyptian state |
| **STC** | Saudi Arabia | ~64% | PIF |
| **Etisalat / e&** | UAE | ~60% | Emirates Investment Authority |
| **Ooredoo** | Qatar | ~51% | QIA |
| **Orange** | France | ~23% | APE |
| **Telkom SA** | South Africa | ~40% | PIC/GEPF |
| **Türk Telekom / Turkcell** | Turkey | ~26% each | Turkey Wealth Fund |
| **Telekom Malaysia** | Malaysia | ~25% | Khazanah |

Fully privatized (for contrast): **Telstra** (Australia), **BT** (UK), **MTS** (Russia, private/Sistema), Magyar Telekom (DT-owned).

## Sovereign wealth funds as telecom owners

Several SWFs hold stakes across **multiple** carriers simultaneously, amplifying state reach: **Temasek** (SingTel, Bharti Airtel), **PIF** (STC + Bahrain/Kuwait arms), **QIA** (Ooredoo, Vodafone Qatar), **KIA** (Zain), **Samruk-Kazyna** (KazakhTelecom), **Khazanah** (TM, Axiata), **TVF** (Türk Telekom + Turkcell), **Ministry of SOEs/BUMN** (Telkom Indonesia, Telkomsel), plus the European state vehicles KfW, APE, CDP, ÖBAG.

## Why ownership = control

A government that owns the major carrier gains built-in **lawful intercept**, national-level **content blocking** (BGP blackhole / DNS), backbone **DPI monitoring**, instant **internet shutdown** (order the carrier to withdraw routes), and **BGP manipulation**. Ease scales with market structure:

| Structure | Examples | Censorship ease |
|---|---|---|
| Full monopoly | Cuba, North Korea, Turkmenistan | trivial — one instruction |
| State duopoly | Ethiopia (pre-2021), Belarus | easy |
| State majority / controls gateways | China, UAE | easy |
| State partial + regulated private | Saudi Arabia, Egypt | moderate (via regulator) |
| Privatized but regulated | UK, Germany, France | hard (needs court orders) |
| Private, light regulation | USA, Canada | hardest |

## Notable profiles

- **China Telecom** (AS4809/CN2 premium backbone): all international traffic routes through six state-controlled gateways; repeatedly accused of **BGP hijacks** (2010/2019/2022); FCC revoked its US license in 2022.
- **Viettel** (Vietnam MoD): a military-owned carrier with commercial operations in 10+ countries (Cambodia, Mozambique, Peru, Myanmar…).
- **Rostelecom** (Russia): operates mandatory **SORM** DPI giving the FSB real-time access; core of the "Sovereign Internet" (RuNet); de-peered by Cogent and Lumen in 2022.
- **North Korea (KPTC)**: ~5 BGP prefixes total; global internet limited to elites, the rest on the **Kwangmyong** intranet; upstream via China Unicom + Rostelecom.
- **ETECSA (Cuba)**: ALBA-1 cable to Venezuela; public mobile internet only since Dec 2018; among the world's slowest/most expensive.

## Privatization history

Many Tier-1/2 ISPs began as state **PTT** monopolies privatized 1984–2010 — **BT** (1984, the first), then partial IPOs keeping a state stake (**NTT** 33%, **DT** 32%, **Orange** 23%, **Telstra** 1997–2006 full, **Telkom SA** ~40%), and full sales (Telefónica, Telecom Italia, **Tata/VSNL** 2002, KT 2002, PTCL→Etisalat 2006).

Related: [[Network Infrastructure]] · [[Submarine Cables]] · [[Network Infrastructure#4. Governance — who hands out the numbers|internet governance]] · [[05 Networking|domain overview]]
