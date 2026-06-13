---
type: note
tags: [networking, infrastructure, regional, latin-america]
---


RIR: **LACNIC**. The market is shaped less by individual countries than by a handful of **transnational carrier groups** (below), with **Brazil** as the regional hub. (Part of [[Network Infrastructure]].)

## The carrier groups that define the region

| Group | Brand | Footprint | Parent |
|---|---|---|---|
| **América Móvil** | Claro | ~all of LatAm, 30–40% share each | Carlos Slim (Mexico) |
| **Telefónica** | Movistar/Vivo | Brazil, Southern Cone, Andes | Telefónica (Spain) |
| **Millicom** | Tigo | Bolivia, Paraguay, Central America | Millicom (Sweden/Lux) |
| **Liberty Latin America** | Flow/VTR | Caribbean, Chile, Costa Rica | LILA (NASDAQ) |
| **Digicel** | — | Caribbean, Haiti, Jamaica | Denis O'Brien (Ireland) |
| **Viettel** | Bitel | Peru, Haiti, East Timor | Vietnam MoD (military) |

Several countries retain **state carriers**: ANTEL (Uruguay), CANTV (Venezuela), CNT (Ecuador), ENTEL (Bolivia), ICE (Costa Rica), ETECSA (Cuba), plus municipal telcos ETB (Bogotá) and EPM (Medellín).

## Brazil — the regional hub

Penetration ~84%, ~180M users (5th-largest market globally); regulator **ANATEL**. Its standout asset is **IX.br** (run by non-profit NIC.br): **IX.br São Paulo peaks at ~17 Tbps with 3,000+ members — the largest IXP in the Southern Hemisphere** — and the model operates low-cost IXPs across 40+ cities, making Brazil one of the most interconnected ecosystems on Earth relative to population. ISPs: Claro (AS4230, ~30%), Vivo/Telefônica (~25%), TIM, and the remnants of **Oi** (largest corporate bankruptcy in Brazilian history, 2016, R$65B → mobile sold to the big three 2022, fixed → V.tal wholesale fiber). All hyperscalers have **São Paulo** regions (AWS sa-east-1 — the only AWS region in South America — plus Azure, GCP, Oracle); cables land at **Fortaleza** (EllaLink to Portugal, SACS/SAIL to Africa, GlobeNet, AMX-1) and Santos (Firmina). Regulation is globally influential: the **Marco Civil** "internet constitution" (2014) and **LGPD** (GDPR-equivalent, 2020); WhatsApp (~120M users) is the dominant platform.

## Southern Cone & Andes

- **Argentina** (~88%, ENACOM): Telecom Argentina (merged with Cablevisión/Fibertel), Claro, Movistar; **CABASE** IXPs (~1.5 Tbps); Google's **Firmina** lands at Las Toninas; currency controls complicate tech ops; Mercado Libre is LatAm's e-commerce giant.
- **Chile** (~90%, SUBTEL): Entel, VTR, Claro, Movistar; **Valparaíso** is a key Google landing (**Curie** to the US West Coast); chosen for Pacific cables thanks to stability; **Google Cloud southamerica-west1 (Santiago, 2021)**.
- **Colombia** (~75%, CRC): Claro (~35%), Movistar, plus municipal **ETB** (Bogotá) and **EPM** (Medellín); NAP Colombia IXP.
- **Peru** (~72%, OSIPTEL): Movistar, Claro, **Bitel (Viettel — Vietnam's military carrier)**; Curie lands near Lima.
- **Uruguay** (~90%, highest in S. America): state **ANTEL** built ~70%+ FTTH — among the region's best networks.
- **Bolivia/Paraguay**: landlocked, state carriers (ENTEL/Copaco) + Tigo; Bolivia has no submarine access (terrestrial links only).

## Venezuela & Cuba — degraded/controlled

**Venezuela** (~72%, declining): state **CANTV** (renationalized 2007) suffers chronic underinvestment and outages; CONATEL blocks social media during opposition events; the **ALBA-1** cable (2013, to Cuba) was the chavista-funded link. **Cuba**: **ETECSA** monopoly, lone **ALBA-1** cable, public mobile internet only since Dec 2018, ~$7/GB, social media blocked during protests.

## Caribbean & Central America

Caribbean connectivity runs on **Liberty Latin America (Flow)** and **Digicel**, plus state telcos (TSTT Trinidad 51%, BTC Bahamas 51%, SETAR Aruba); Haiti has the region's weakest connectivity. Cables: ARCOS, Americas-II, AMX-1, CBUS. In **Central America**, **Tigo** and **Claro** dominate, with state holdouts (ICE Costa Rica, Hondutel, BTL Belize). **Panama** is the regional transit hub — dollar economy, stable governance, regional PoPs for Lumen/Cogent/AWS, and Google's **Curie** transits it linking North and South America.

Related: [[Network Infrastructure]] · [[North-America Digital Infrastructure]] · [[State-Owned & Government-Linked Carriers]] · [[Submarine Cables]] · [[05 Networking|domain overview]]
