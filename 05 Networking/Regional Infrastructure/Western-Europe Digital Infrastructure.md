---
type: note
tags: [networking, infrastructure, regional, western-europe]
---


RIR: **RIPE NCC**. Home to the **"FLAP-D" core** — Frankfurt, London, Amsterdam, Paris + Dublin — the continent's internet centre of gravity, with the world's two busiest IXPs and most US-tech EU headquarters. (Part of [[Internet & Infrastructure]].)

## The FLAP-D hubs

| Hub | Defining asset |
|---|---|
| **Frankfurt** | **DE-CIX (~14 Tbps, 1,000+ members) — the world's busiest IXP**; ECB/Eurex + European HFT; densest DC cluster on the continent |
| **London** | **LINX (~8 Tbps, 900+)**; Telehouse Docklands; densest trans-Atlantic landings in Europe; financial-services cluster |
| **Amsterdam** | **AMS-IX (~10 Tbps, 900+)**; RIPE NCC HQ; Google/Meta/Microsoft hyperscale DCs in Flevoland |
| **Paris/Marseille** | France-IX; **Marseille** is Europe's gateway for Africa/Middle-East/Asia cables (all SMW, AAE-1, Medusa land here) |
| **Dublin** | EU HQ for Apple/Google/Meta/Stripe (12.5% tax); AWS's **primary EU region** (eu-west-1) |

## United Kingdom

Penetration ~97%; regulator **Ofcom**; still RIPE post-Brexit. ISPs: **BT** (AS2856, ~35%, with **Openreach** functionally separated 2017 for equal wholesale access), Virgin Media (Liberty Global), Sky (Comcast), TalkTalk; mobile EE(BT)/Vodafone/O2/Three. Clouds: AWS eu-west-2, Azure UK South/West, GCP europe-west2. Cable landings on the **Cornwall/Somerset coast** (Bude — Dunant/FLAG; Highbridge — Amitié), historically tapped by GCHQ (TEMPORA, Five Eyes). Regulation: Telecoms Security Act 2021, Online Safety Act 2023, Huawei 5G ban (full removal by 2027).

## Germany

Penetration ~96%; regulator **BNetzA**; **strongest data protection in the EU** (aggressive state DPAs). **DE-CIX Frankfurt** is the anchor; ISPs led by **Deutsche Telekom** (AS3320, ~40%, German state ~32% via KfW — and globally **owns ~48% of T-Mobile US**, ~115M subs), Vodafone Germany, 1&1/United Internet, O2. Frankfurt hosts the densest Equinix/Digital Realty footprint plus AWS eu-central-1, Azure, GCP europe-west3. No ocean landings (capacity arrives terrestrially from UK/NL). Lagging on fiber (~15% FTTH vs 80%+ Nordics); never fully banned Huawei.

## France

Penetration ~94%; regulator **ARCEP**, watchdog **CNIL** (aggressive — fined Google €150M, Meta €60M in 2022). ISPs: **Orange** (AS3215, ~40%, French state ~23%), SFR (Altice), Bouygues, and disruptor **Free/Iliad** (Xavier Niel, who slashed prices from 2002). **Marseille** is the critical Mediterranean cable hub (MRS1–3); **OVHcloud** (Roubaix) is Europe's largest cloud/hosting provider; AWS eu-west-3, Azure, GCP europe-west9, plus sovereign **Scaleway**. Notable: the 2021 OVH Strasbourg fire (3.6M sites offline) and the "Cloud de confiance"/SecNumCloud sovereignty push.

## Netherlands, Belgium, Luxembourg

- **Netherlands** (~98%, ACM): **AMS-IX** + RIPE NCC HQ make Amsterdam a governance+peering hub; KPN (fully private), VodafoneZiggo; GCP europe-west4 (Eemshaven), Azure, and **Meta hyperscale campuses** in Flevoland; Euronext/HFT cluster.
- **Belgium** (~95%, BIPT): **Proximus** (Belgian state ~53%), Telenet (Liberty Global); **Google's first European cloud region** at Saint-Ghislain, where the **Dunant** cable lands (terrestrial from Bude).
- **Luxembourg** (~99%): 100% state **POST Luxembourg**; govt-backed LuxConnect DCs; Amazon's European HQ (low tax) and EU domicile for several Big Tech entities.

## Switzerland, Austria, Ireland

- **Switzerland** (~97%, BAKOM, non-EU): **Swisscom** (Confederation ~51%), Sunrise, Salt (Xavier Niel); CIXP at CERN; AWS eu-central-2 + Azure + GCP all in Zurich; strict privacy (nDSG) makes it a **data-sovereignty** haven.
- **Austria** (~95%, RTR): **A1/Telekom Austria** (América Móvil ~51%, state ~28%) — notable as the **backbone operator across 8 CEE/Balkan countries**; VIX Vienna IXP.
- **Ireland** (~95%, ComReg): disproportionately important as the **EU HQ hub for US tech**; **eir** (Iliad), Virgin, Sky; **AWS's first EU region (2007)**; Meta (Clonee), Apple (Athenry), Microsoft campuses. Its **DPC** is the lead EU regulator for all those firms — criticized as slow but issuing landmark fines (Meta €1.2B 2023, Instagram €405M, TikTok €345M).

Related: [[Internet & Infrastructure]] · [[Northern-Europe Digital Infrastructure]] · [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
