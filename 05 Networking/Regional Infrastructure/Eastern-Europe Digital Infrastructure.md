---
type: note
tags: [networking, infrastructure, regional, eastern-europe]
---


RIR: **RIPE NCC**. EU members + the post-Soviet borderlands, where the story is **Deutsche Telekom's regional dominance**, fast cheap fiber, emerging hyperscale DC hubs, and a sharp resilience contrast between Ukraine and Belarus. (Part of [[Network Infrastructure]].)

## Poland — the regional hub

Penetration ~90%, ~38M people (the largest EE market); regulator **UKE**. **PLIX (Warsaw, ~2.5 Tbps)** is a top-10 European IXP. ISPs: **Orange Polska** (AS5617), billionaire **Zygmunt Solorz's Cyfrowy Polsat** (Plus mobile + Netia fixed), and **Play** (now Iliad/Xavier Niel, which also absorbed UPC Polska). All three hyperscalers built dedicated **Warsaw cloud regions** (GCP europe-central2, Azure Poland, AWS Local Zone) — cheap land/energy + EU membership. NATO's most active eastern member and an early Huawei hawk (arrested a Huawei employee on espionage charges in 2019).

## Central Europe — Deutsche Telekom country

DT owns the incumbent across much of the region (the largest single-operator footprint), so a large share of Central/SE European traffic ultimately transits its backbone:

| Country | Incumbent | Note |
|---|---|---|
| **Czech** (~93%, ČTÚ) | T-Mobile CZ (DT) | but **O2 Czech = PPF** (Kellner estate, which also held Telenor Serbia); **NIX.CZ** ~1.2 Tbps |
| **Slovakia** (~89%) | **Slovak Telekom (DT ~100%)** | many enterprises co-locate in nearby Vienna |
| **Hungary** (~90%, NMHH) | **Magyar Telekom (DT ~100%)** | Orbán-era media concentration, but ISPs uncensored; didn't ban Huawei |

## Romania & Bulgaria

- **Romania** (~87%, ANCOM): a **speed paradox** — among the world's fastest internet (avg >200 Mbps) despite being a poorer EU state, because **Digi (RCS&RDS)** blanket-deployed apartment-block fiber in the 2000s (now 1 Gbps for ~€10/mo and expanding into Spain/Portugal/Belgium). RONIX IXP; AWS announced a **Bucharest region**.
- **Bulgaria** (~85%, CRC): **Vivacom** (United Group), A1, Yettel; slower to act on Huawei; heavy Russian energy ties.

## Ukraine vs Belarus — the resilience contrast

This is the region's key lesson in internet architecture.

**Ukraine** (~78%, distributed): a **deliberately decentralized** internet — many ISPs (Kyivstar/VEON, Vodafone Ukraine, Ukrtelecom, Lifecell), **UA-IX in Kyiv (~2 Tbps)**, and multiple border crossings (Poland, Slovakia, Romania, Moldova) none of which Russia controls. Since Feb 2022 it has **withstood physical fiber cuts and DDoS** by rerouting; **~20,000+ Starlink terminals** kept military/civilian comms alive where fiber was cut — the canonical proof of LEO satellite strategic value. Occupied areas (Crimea since 2014, Donbas/Kherson since 2022) were force-rerouted through **Rostelecom/Miranda-Media**.

**Belarus** (~83%, authoritarian): the opposite — **Beltelecom (state ~100%) is the sole international gateway**, making total control trivial. During the disputed Aug 2020 election it imposed a **near-total shutdown** (~95% connectivity loss); opposition sites permanently blocked; SORM-style surveillance with Russian help; increasingly integrated into Russia's RuNet.

**Moldova** (~76%, EU candidate since 2022): state **Moldtelecom** + Orange/Vodafone; complicated by the Russian-backed **Transnistria** breakaway region; rerouting away from Russia toward Romania/Ukraine.

## Cross-cutting themes

**China's BRI digital reach** — Serbia (Huawei "Safe City"), Hungary (Huawei 5G exempted) accept Chinese infrastructure, seen as intelligence risk by NATO. **Emerging DC hubs** — Warsaw and Bucharest now host hyperscaler regions, drawing investment from pricier Western cities via EU funds + green energy + cheaper land.

Related: [[Network Infrastructure]] · [[Russia-Cis Digital Infrastructure]] · [[Northern-Europe Digital Infrastructure]] · [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
