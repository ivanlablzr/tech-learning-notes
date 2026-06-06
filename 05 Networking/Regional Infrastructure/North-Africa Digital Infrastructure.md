---
type: note
tags: [networking, infrastructure, regional, north-africa]
---


RIR: **RIPE NCC** (a quirk — North Africa is administered by the European registry, not AFRINIC). The region is dominated by one fact — **Egypt's Suez chokepoint** — alongside heavy state control and chronic under-provisioning by hyperscalers. (Part of [[Internet & Infrastructure]].)

## Egypt — the global chokepoint

Penetration ~72%, 106M people; regulator **NTRA**. **Telecom Egypt** (AS8452, state) holds a legal monopoly on submarine-cable landing rights and the international gateway, giving the state full visibility/control. **~17% of all global internet traffic transits Egypt**: cables land at **Alexandria**, cross ~170 km of land to **Suez**, and re-enter the Red Sea — 11+ systems (SMW3/4/5/6, AAE-1, IMEWE, EIG, ACE, 2Africa, Blue-Raman). It's the single most important terrestrial chokepoint on Earth, and a unique geopolitical lever (alternatives — South Africa route +120 ms, satellite, Russia-controlled Trans-Siberian — are all worse).

Egypt also demonstrated the **kill switch**: during the 2011 Arab Spring it executed a **near-total 5-day shutdown** (TE withdrew all Egyptian BGP prefixes, mobile radios ordered off, ~93M disconnected); "kill switch" capability and SORM-style monitoring interfaces are now enshrined in law.

## The Maghreb

| Country | Penetration | Structure |
|---|---|---|
| **Morocco** (~88%, ANRT) | **Maroc Telecom** (e&/UAE ~60%, state ~40%) + INWI, Orange | **Casablanca** is the West-Africa→Europe cable gateway (ACE, 2Africa, Atlas Offshore); relatively open |
| **Algeria** (~71%, ARPT) | **Algérie Télécom** (100% state) monopoly + Djezzy/Ooredoo | **no neutral IXP**, scarce/expensive bandwidth, **annual exam-time nationwide blackouts**, illegal VPNs, Hirak-era throttling |
| **Tunisia** (~79%, INT) | Tunisie Télécom (state 75%) + Ooredoo/Orange | post-2011 it dismantled the notorious ATI filtering agency; filtering has crept back since the 2021 Saied consolidation |

## The fragile states

- **Libya** (~52%): infrastructure shattered by civil conflict; **split between Tripoli and Benghazi administrations** with separate networks; no functional IXP, undersized cables, frequent outages.
- **Sudan** (~30→~20%): state **Sudatel** monopoly; repeated shutdowns (2019 revolution, 2021 coup); the **2023 SAF–RSF war** destroyed much of Khartoum's infrastructure (Starlink filling gaps unofficially).
- **Mauritania** (~42%): low penetration from infrastructure gaps (one cable, ACE at Nouakchott) rather than censorship — relatively open.

## The hyperscaler gap

North Africa is **badly underserved by cloud**: **no AWS or Azure region anywhere** in the region (nearest are the Gulf or Europe), and Google/Microsoft only run edge caches. Cloud-native workloads pay **+60–120 ms** latency, a real handicap for the regional tech ecosystem. State control runs from chokepoint-monopoly (Egypt, Algeria) → high (Sudan) → moderate (Morocco, Tunisia) → low (Mauritania) → fragmented/war (Libya).

Related: [[Internet & Infrastructure]] · [[Middle-East Digital Infrastructure]] · [[Sub-Saharan-Africa Digital Infrastructure]] · [[Submarine Cables]] · [[05 Networking|domain overview]]
