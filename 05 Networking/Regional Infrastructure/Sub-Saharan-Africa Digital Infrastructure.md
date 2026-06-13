---
type: note
tags: [networking, infrastructure, regional, sub-saharan-africa]
---


RIR: **AFRINIC**. ~50 countries, mobile-first, and **cable-led** — international capacity has grown ~100× since 2010 while last-mile, power, and neutral exchanges lag. The story is a few hub nations, a handful of pan-African operators, and the transformational **2Africa** cable. (Part of [[Network Infrastructure]].)

## The hub nations

| Country | Penetration | Why it's a hub |
|---|---|---|
| **South Africa** | ~72% | the continental hub — **NAPAfrica Johannesburg >1 Tbps (busiest IXP in Africa)**, **Teraco** (Africa's neutral DC operator), and **all three hyperscalers** (AWS af-south-1 2020, Azure, GCP africa-south1) |
| **Nigeria** | ~55%, 220M | West Africa's giant — IXPN Lagos, **GCP me-west1 Lagos (first W. African cloud region, 2023)**, MainOne (bought by Meta), and Globacom's **GLO-1, the only African-owned T1-class cable** |
| **Kenya** | ~85% | East Africa's gateway — **Safaricom** (state ~35% + Vodacom; runs **M-Pesa**, ~70% share), KIXP (est. 2000), Mombasa cable landing feeds the landlocked interior |

South Africa serves the whole continent's cloud workloads; East/West African traffic still backhauls there (Nairobi→Cape Town +50 ms; →Bahrain +120 ms), which is exactly why the new Lagos/Nairobi regions matter.

## Pan-African operators

A few groups span the continent: **MTN** (South African — Nigeria, Ghana, Uganda, etc.), **Airtel** (Bharti/India), **Vodacom/Vodafone**, **Orange** (Francophone West/Central Africa), **Liquid Intelligent Technologies** (Econet/Zimbabwe — the dominant cross-border **fiber backbone** for the landlocked interior), and submarine operator **SEACOM**. State incumbents (Telkom SA, Ethio Telecom, TTCL, ZAMTEL) persist but are mostly shrinking.

## Submarine cables — the lifeline

International capacity is almost entirely undersea. Top landings: **South Africa** (Cape Town/Durban, 10 systems), **Nigeria** (Lagos, 7), **Kenya** (Mombasa, 6), **Ghana** (Accra, 6). Key systems: WACS, ACE, SEACOM, EASSy (the older backbones), **SACS** (first South Atlantic, SA↔Brazil), Google **Equiano** (2022), and the transformational **2Africa**.

> **Djibouti** (pop. ~1M) is a strategic outlier — **11+ cables** land there thanks to its position at the **Bab-el-Mandeb** strait (mouth of the Red Sea), plus the US Camp Lemonnier base; state Djibouti Telecom controls every landing — major geopolitical leverage.

> **2Africa** (Meta-led, 180 Tbps, lands in **33 African countries**, ~2024–25): will roughly **triple** Africa's cable capacity, end West/East African bandwidth scarcity, and cut wholesale prices 60–80%. The remaining bottlenecks are last-mile, regulation, and **electricity reliability** (DCs need power).

## Internet shutdowns & control

Shutdowns are a regional signature. **Ethiopia** held the **Tigray region offline for 17+ months** (2020–22, among the longest ever) and broke Africa's **last full telecom monopoly** (Ethio Telecom) only in 2021 (Safaricom Ethiopia). Others: Nigeria's **7-month Twitter ban** (2021), Uganda's election social-media shutdown + "OTT tax," Zimbabwe's 2019 blackout, plus exam-time blocks. Control spectrum: **open** (South Africa, Ghana, Kenya, Senegal, Botswana, Mauritius) → **moderate** (Nigeria, Tanzania, Uganda, Zambia) → **high** (Ethiopia, Cameroon, Zimbabwe, DRC, Sudan) → **severe/war** (Eritrea, Somalia, South Sudan, CAR, Chad).

## The connectivity gap

Despite progress, the divide is stark: SSA median mobile speed ~22 Mbps (vs ~72 global), fixed ~14 Mbps (vs ~84); **~25 of ~50 nations have no neutral IXP**; only **2 countries host a hyperscaler cloud region**; cable capacity per capita is ~**1:40 vs Europe**. Bright spot: **Cloudflare's** aggressive edge expansion puts ~65% of the population within 50 ms of a CDN PoP. Connectivity tiers: **hub** (South Africa, Nigeria, Kenya) → regional (Ghana, Senegal, Côte d'Ivoire, Tanzania) → developing (Uganda, Rwanda, Angola, Zambia) → minimal (DRC, much of Central Africa) → crisis (Somalia, South Sudan, CAR).

Related: [[Network Infrastructure]] · [[North-Africa Digital Infrastructure]] · [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
