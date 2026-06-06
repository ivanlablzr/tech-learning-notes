---
type: note
tags: [networking, infrastructure, regional, southern-europe, balkans]
---


RIR: **RIPE NCC**. The Mediterranean rim and the Balkans — defined by **cable landing hubs** (the gateway between Europe and Africa/Asia) and, in the Balkans, by a few foreign carrier groups owning almost everything. (Part of [[Internet & Infrastructure]].)

## Mediterranean cable hubs

| Hub | Role |
|---|---|
| **Milan** | Southern-EU crossroads — MIX IXP (~2 Tbps), TI Sparkle backbone, AWS/Azure/GCP |
| **Marseille** | the big Mediterranean landing (shared with France) — Africa/ME/Asia cables |
| **Palermo/Sicily** | TI Sparkle landings for SMW3/4, Medusa, IMEWE — Italy gatekeeps much Europe–Asia traffic |
| **Lisbon/Sines** | Atlantic hub — EllaLink (Brazil), Grace Hopper (Google/US), Equiano (Africa) |
| **Madrid** | Iberian + LatAm gateway; Tarifa is the ~14 km Gibraltar crossing to Morocco |
| **Cyprus/Athens** | eastern-Med node into the Suez/Middle-East systems |

## Italy

Penetration ~85%; regulator **AGCOM**. **Telecom Italia Sparkle** (AS6762) is a genuine **Tier-1** backbone and major submarine-cable operator. The market is dominated by **TIM** (in long governance turmoil since Vivendi 2015 — **KKR bought its fixed network "NetCo" for €22B**), Fastweb (Swisscom), WindTre (CK Hutchison), Vodafone. MIX Milan is the anchor IXP; AWS eu-south-1, Azure, GCP europe-west8 all in Milan; state-funded **Open Fiber** drives FTTH.

## Spain

Penetration ~94%; regulator **CNMC**. **Telefónica** (AS12956, state ~10%) is a global heavyweight (~300M customers across Europe + Latin America via Movistar/Vivo/O2). Competitors: Vodafone, Orange, and the merged **Orange+MásMóvil** (2024). ESPANIX (Madrid) + CATNIX (Barcelona); AWS eu-south-2 (Aragón), Azure Madrid, GCP europe-southwest1. Atlantic landings at Sopelana/Bilbao (**Marea**); **Tarifa** anchors the Europe–Africa crossing at Gibraltar.

## Portugal & Greece

- **Portugal** (~84%, ANACOM): NOS and **Altice/MEO** split the market; **Sines** has become a major Atlantic cable hub (EllaLink, Grace Hopper, Equiano all land there).
- **Greece** (~84%, EETT): **OTE/Cosmote** (Deutsche Telekom ~48%, Greek state ~34%), Vodafone, Nova/Wind (United Group); GR-IX Athens; Aegean waters are a sensitive cable corridor amid Greece–Turkey tensions.

## The Balkans & Med islands — owned by a few groups

A handful of carrier groups own the regional incumbents, which makes the ownership map the real story:

| Group | Footprint |
|---|---|
| **Deutsche Telekom** | Croatia (HT 51%), N. Macedonia (51%), Montenegro (77%), + OTE Greece |
| **Telekom Austria (A1)** | Croatia, Slovenia, Serbia, N. Macedonia, Kosovo, Belarus |
| **United Group** (BC Partners) | Serbia (SBB), Croatia (Telemach), Slovenia, Greece (Nova/Wind) — pan-Balkan cable/broadband |
| **Telekom Slovenije** | N. Macedonia, Kosovo (IPKO), Albania |

State telcos persist: **Telekom Srbija** (Serbia ~58%), **Telekom Slovenije** (~62%), **BH Telecom**/m:tel (Bosnia, split by the Dayton entity structure), **Cyta** (Cyprus, ~100% state), GO (Malta).

**Geopolitical flashpoints:** **Serbia** — EU candidate but close to Russia/China, hosts Huawei "Safe City" surveillance under BRI and hasn't joined Russia sanctions. **Kosovo** — no UN recognition, complex IP/ccTLD situation (informal .xk). **Cyprus** — divided (the North on Türk Telekom), an eastern-Med cable node, and a financial hub with governance concerns. **Malta** — a "Blockchain Island"/iGaming data hub. **Montenegro** — its **.me** ccTLD is a global money-spinner.

Related: [[Internet & Infrastructure]] · [[Western-Europe Digital Infrastructure]] · [[Eastern-Europe Digital Infrastructure]] · [[Submarine Cables]] · [[05 Networking|domain overview]]
