---
type: note
tags: [networking, infrastructure, submarine-cables]
---


~**99% of intercontinental internet traffic** rides submarine fiber (satellites carry <1%). As of 2024 there are ~600 active or planned systems. They are the physical substrate of [[Network Infrastructure]] and an increasingly strategic vulnerability.

## How they work

A cable links two **landing stations** across the seabed: 4–16 **fiber pairs** (one fiber each direction), with optical **repeaters** every ~50–100 km powered by up to 15,000 V DC fed from shore. **WDM** packs 100+ wavelengths per fiber (100–400 Gbps each) for total capacity of 20–400+ Tbps. Landing stations are secure, often militarily significant facilities. Ownership is **consortium** (carriers share cost/capacity via IRUs), **private** (one hyperscaler funds and owns it all), or **hybrid** (an anchor like Google funds the majority, others buy IRU capacity).

## Trans-Atlantic

| Cable | RFS | Cap. | Owners |
|---|---|---|---|
| **Amitié** | 2022 | 400 Tbps | Microsoft, Meta, Aqua Comms |
| **Grace Hopper** | 2022 | 352 Tbps | Google (sole) — NY↔UK+Spain |
| **Firmina** | 2023 | 352 Tbps | Google — US↔S. America |
| **Dunant** | 2020 | 250 Tbps | Google (sole) |
| **Havfrue/AEC-2** | 2020 | 264 Tbps | Google, Meta, Aqua Comms |
| **Marea** | 2018 | 200 Tbps | Microsoft, Meta, Telxius |
| **EllaLink** | 2021 | 72 Tbps | consortium — Brazil↔Portugal |
| **Hibernia Express** | 2015 | 8.4 Tbps | GTT — lowest latency (~58 ms NY–London) |

Older TAT-14/Apollo/FLAG Atlantic-1/AC-1/AC-2 (2001-era, sub-5 Tbps) remain but are aging. US landings cluster at Virginia Beach, Tuckerton (NJ), Shirley (NY); European at Bude (UK), Sopelana (Spain), Brittany (France).

## Trans-Pacific

| Cable | RFS | Cap. | Owners |
|---|---|---|---|
| **Echo** | 2024 | 200+ Tbps | Google, Meta — US↔Singapore/India |
| **Bifrost** | 2024 | 200+ Tbps | Meta, Telin — US↔Indonesia via Guam |
| **SJC2** | 2022 | 126 Tbps | Google, Meta, SoftBank, PLDT… |
| **Pacific Light (PLC)** | 2022 | 144 Tbps | Google, Meta — **HK segment blocked by US FCC (2021)** |
| **NCP** | 2018 | 80 Tbps | China Telecom/Unicom/Mobile, KT, Microsoft |
| **Curie** | 2019 | 72 Tbps | Google — LA↔Chile/Peru |
| **FASTER / JUPITER** | 2016/20 | 60 Tbps | Google/Amazon/Meta + Asian carriers |

US Pacific landings: LA, Hermosa Beach, Hillsboro/Nedonna (OR); Japan: Chikura, Shima, Maruyama. US FCC/DOJ security reviews increasingly block China/HK endpoints.

## Europe–Asia (via Red Sea / Indian Ocean)

| Cable | RFS | Cap. | Owners |
|---|---|---|---|
| **Blue-Raman** | 2024 | 360 Tbps | Google — Italy↔India via Saudi Arabia |
| **SEA-ME-WE 6** | 2025 | 100+ Tbps | Google anchor + consortium |
| **2Africa** (segment) | 2024 | 180 Tbps | Meta-led — circles Africa |
| **PEACE** | 2022 | 96 Tbps | China-backed — Pakistan↔Kenya↔France |
| **AAE-1** | 2017 | 40 Tbps | multi-carrier Asia↔Africa↔Europe |
| SMW3/4/5 | 1999–2016 | 1–24 Tbps | large aging consortia |

## Africa

**West (Atlantic):** **2Africa** (2024, 45,000 km, 180 Tbps — Meta-led, the largest cable ever, circles the continent and roughly doubles Africa's bandwidth), **Equiano** (2023, 144 Tbps, Google), ACE/WACS (2012, Orange/consortium), SACS/SAIL (Angola/Brazil). **East (Indian Ocean):** EASSy, SEACOM, TEAMS, LION/LION2, DARE1 (2024), Google **Umoja** (Kenya↔Spain).

## Other regional systems

**Intra-Asia/Pacific:** APG, AJC, JGA, Indigo, plus government-funded **Coral Sea Cable** (Australia→PNG/Solomons, built specifically to pre-empt a Chinese-funded cable). **Arctic** (Far North Fiber, est. 2026): a potential 3rd Europe–Asia route as the NW Passage opens. **Mediterranean:** Medusa, BlueMed (TI Sparkle rings).

## Hyperscaler dominance & geopolitics

Private investment flipped cable economics after 2016 — **~70% of new cable capacity (2016–2024) is hyperscaler-funded**: Google (15+ cables), Meta (8+), Amazon and Microsoft (2–5 each). Consequences: cables optimized for **datacenter-to-datacenter** (not ISP) traffic, preferential/exclusive capacity for the owner, eroded carrier leverage, and US government scrutiny of any cable landing in China/HK.

**Chokepoints:** the **Red Sea/Suez** corridor carries ~17% of global traffic (SMW3/4/5/6, AAE-1, EIG, Blue-Raman, PEACE) — multiple cuts in 2024 (Houthi-linked anchor drags); the **Strait of Malacca** (2.8 km wide) and **Luzon Strait** (earthquake-prone) funnel most Asia-Pacific cables.

**Cuts are common** — ~67% from fishing gear, ~15% ship anchors, ~10% natural (quakes/landslides), ~2% deliberate sabotage. Notable: 2006 Taiwan quake (7 cables, SE Asia −50–80% for weeks), 2022 Tonga eruption (5-week blackout), 2024 Red Sea + Baltic cuts (suspected sabotage). Repairs take 1–4 weeks via ~60 global cable ships (ASN, SubCom, NEC, HMN).

**Landing-station security** is treated as critical national infrastructure: US CFIUS/Team Telecom reviews, UK Telecoms Security Act 2021, France's "Operator of Vital Importance" designation. The 2020 FCC rejection of Pacific Light's HK landing and Australia's Coral Sea Cable are the template decisions.

Related: [[Network Infrastructure]] · [[State-Owned & Government-Linked Carriers]] · [[Space & Satellite Networks]] · [[05 Networking|domain overview]]
