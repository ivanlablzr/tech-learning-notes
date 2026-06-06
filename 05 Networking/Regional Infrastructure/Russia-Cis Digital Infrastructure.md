---
type: note
tags: [networking, infrastructure, regional, russia, central-asia]
---


RIR: **RIPE NCC**. Russia + the post-Soviet states — the region defined by **state control of the internet** (the world's most sophisticated outside China) and, since 2022, accelerating **decoupling** from the global internet. (Part of [[Internet & Infrastructure]].)

## Russia

Penetration ~89%, ~100M users (Europe's largest by users); regulator **Roskomnadzor**. **MSK-IX** (Moscow, ~6 Tbps) is the largest IXP in Eastern Europe. The market: state-influenced **Rostelecom** (AS12389, ~38% state — operates all SORM/TSPU) plus the "Big Four" mobile carriers **MTS** (Sistema), **Beeline** (VEON), **MegaFon** (Usmanov-linked), **Tele2** (Rostelecom). Notable transit: **TransTeleCom (TTK)** runs ~85,000 km of fiber along the railway network.

**The control stack** — the most capable national system outside China:
- **SORM** — mandatory FSB interception hardware at every ISP (gen 1 voice → 2 internet → 3 metadata).
- **Sovereign Internet law (90-FZ, 2019)** — **TSPU** DPI boxes at all ISPs that Roskomnadzor can remotely use to filter/reroute; BGP can be forced through Rostelecom; a domestic **RuNet DNS** root lets Russia run disconnected from ICANN (tested in annual "sovereign internet" drills).
- **Blocks:** Meta (ruled "extremist"), Twitter/X, BBC, LinkedIn (since 2016), 500+ sites, many VPNs. The 2018 **Telegram** block backfired (collateral-blocked millions of AWS/Google IPs) and was lifted in 2020.

**Post-2022 decoupling:** Cogent and Lumen withdrew (lost trans-Atlantic transit); AWS/Azure/GCP suspended Russian sales → forced domestic cloud (Yandex Cloud, SberCloud, VK Cloud); international routing shifted toward **China Telecom/Unicom**; the Ukraine route was severed.

## Central Asia — state monopolies

The international gateway is almost always a single state carrier, making filtering/shutdown trivial:

| Country | Carrier | Note |
|---|---|---|
| **Kazakhstan** (~91%) | **Kazakhtelecom** (state ~52%) | regional fiber hub Russia↔China; 5-day shutdown during Jan 2022 unrest; became #2 Bitcoin-mining country after China's ban |
| **Uzbekistan** (~80%) | **UzTelecom** (~100%) | sole gateway; the **Telia bribery scandal** ($965M settlement, Karimova) |
| **Turkmenistan** (~35%, lowest) | **Turkmentelecom** (monopoly) | near-North-Korean isolation; ~$1,600/mo for 512 Kbps; Tor/VPN blocked |
| **Kyrgyzstan** (~74%) | KyrgyzTelecom + private | the freest Central-Asian market |
| **Tajikistan** (~35%) | Tajiktelecom | landlocked, expensive transit, election-time shutdowns |

## The Caucasus — contested routing

- **Azerbaijan** (~86%): state **AzTelecom**; controls the BTC pipeline-corridor fiber; cut comms in Nagorno-Karabakh (2020/2023); a **trans-Caspian** fiber node.
- **Armenia** (~79%): Ucom, Beeline (VEON), VivaCell-MTS; after losing Karabakh (2023) it is **cooling toward Russia** and diversifying routing toward the EU/US.
- **Georgia** (~76%): Silknet, Magticom, Caucasus Online — a **critical trans-Caucasus transit** node (Caucasus Cable System + BTC-corridor fiber) linking Azerbaijan/Armenia to Turkey/Europe; relatively open, though the 2024 "foreign agents" law drew protests.

## Cross-cutting

**State-control spectrum** (most → least): Turkmenistan ≈ Russia ≈ Belarus > Kazakhstan ≈ Uzbekistan ≈ Tajikistan > Azerbaijan > Kyrgyzstan ≈ Armenia > Georgia.

**Ownership groups:** **VEON** (Dutch-listed, ex-Russian Alfa; Beeline across Russia/Ukraine/Kazakhstan/Uzbekistan/Armenia/Georgia — Fridman divested amid sanctions), **MTS/Sistema** (Russia, Belarus, Armenia), **Turkcell** (Azerbaijan, Belarus, Georgia), **Rostelecom** (Russia + CIS wholesale). The net post-2022 trend is **CIS states (Kazakhstan, Armenia, Georgia) seeking EU-aligned connectivity** away from Russia.

Related: [[Internet & Infrastructure]] · [[Eastern-Europe Digital Infrastructure]] · [[Middle-East Digital Infrastructure]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
