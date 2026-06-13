---
type: note
tags: [networking, infrastructure, regional, middle-east]
---


RIR: **RIPE NCC**. The world's busiest transit corridor — nearly all Europe↔Asia cables funnel through the **Red Sea → Suez → Gulf** chokepoint — paired with wealthy state-controlled Gulf markets and war-shattered ones. (Part of [[Network Infrastructure]].)

## The cable chokepoint

| Hub | Country | Role |
|---|---|---|
| **Fujairah** | UAE | the primary Europe–Asia nexus (SMW3/4/5, EIG, AAE-1, IMEWE, FALCON) |
| **Jeddah** | Saudi Arabia | Red Sea gateway (SMW6, 2Africa, EIG) |
| **Muscat** | Oman | secondary Asia routing (FALCON, SMW) |
| **Istanbul** | Turkey | the Europe–Asia land bridge + Black Sea/Med junction |

A cut here is global: in early 2024 **Houthi anchor drags severed AAE-1/SEACOM/EIG/TGN in the Red Sea, disrupting ~25% of Europe–Asia traffic** for weeks (repair ships couldn't safely enter).

## The Gulf — wealthy, concentrated, state-run

| Country | Penetration | Operators (ownership) |
|---|---|---|
| **UAE** | ~99% | **duopoly**: e&/Etisalat (govt ~60%) + du (Dubai govt) — VoIP blocked to protect revenue; AWS me-central-1, Azure |
| **Saudi Arabia** | ~96% | **STC** (PIF ~64%, regional player), Mobily, Zain; Azure + **GCP Dammam (2024)**; NEOM mega-build |
| **Qatar** | ~99% | Ooredoo (QIA ~51%); **GCP me-central1 Doha — first ME Google region (2023)** |
| **Bahrain** | ~100% | Batelco (govt ~37%); **AWS me-south-1 — the first ME AWS region (2019)** |
| **Kuwait/Oman** | ~99/95% | Zain/STC/Ooredoo; **Omantel** (OIA ~70%) a transit provider; Oman keeps an Iran back-channel |

These Gulf telcos project regional power: **e&** operates in ~16 countries, **STC** runs Bahrain/Kuwait arms, **Ooredoo** and **Zain** span the Gulf and Africa (see [[State-Owned & Government-Linked Carriers]]).

## Turkey & Israel — the bridge and the cyber power

- **Turkey** (~90%, BTK): at the Europe–Asia crossroads, controls the **Turkish Straits** (Black Sea cable gatekeeper); **Türk Telekom**/Turkcell (both ~26% Turkey Wealth Fund); heavy blocking (500k+ URLs, periodic Twitter/Wikipedia blocks), 2020 social-media localization law; building domestic 5G core (ULAK).
- **Israel** (~92%, "Startup Nation"): Bezeq, Cellcom, Partner, HOT; AWS il-central-1 + Azure (2023). Global outsized influence in security via **Unit 8200** alumni (Check Point, CyberArk, **NSO/Pegasus**, Cellebrite). The **Abraham Accords (2020)** enabled the **REDS** cable (Israel–UAE–Saudi–India).

## Iran & the war-torn states

- **Iran** (~79%, heavily filtered): state **TCI**, Irancell (MTN ~49%); the **National Information Network (SHOMA)** is a genuine *parallel* domestic internet that keeps banking/gov services running during international shutdowns — not just a filter.
- **Iraq** (~80%): among the worst shutdown records — multi-day protest blackouts and **mobile-internet cuts during school exams**; ISIS-era infrastructure damage.
- **Lebanon** (~84%, declining): the economic collapse left state **OGERO** out of generator diesel and engineers; Hezbollah runs a parallel private fiber network.
- **Syria** (~55%) & **Yemen** (~30%): war-devastated state monopolies (STE; TeleYemen), regime internet cuts, and the Yemen-adjacent Red Sea cable cuts above.

## Regulatory spectrum

Free → not free: **Israel** (free) > Jordan/Lebanon/Iraq (partly free) > Turkey/Qatar/Kuwait/Bahrain/Oman (not free, VPN grey) > **UAE/Saudi** (VoIP blocked, business-only VPN) > **Iran/Syria** (very high blocking, VPN illegal). Pegasus-style surveillance is documented across several Gulf states (incl. the Khashoggi case).

Related: [[Network Infrastructure]] · [[North-Africa Digital Infrastructure]] · [[Russia-Cis Digital Infrastructure]] · [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
