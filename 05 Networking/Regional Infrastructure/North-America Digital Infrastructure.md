---
type: note
tags: [networking, infrastructure, regional, north-america]
---


RIR: **ARIN**. The single most important node in global internet infrastructure — most of the world's largest CDNs, clouds, IXPs, and Tier-1 backbones originate here. (Part of [[Network Infrastructure]]; see also [[Submarine Cables]], [[State-Owned & Government-Linked Carriers]].)

## United States

Penetration ~93%; regulator **FCC**, antitrust DOJ+FTC.

**Ashburn, Virginia** — "the internet capital of the world": the highest concentration of data centers on Earth (Equinix alone runs 15 IBX campuses in Loudoun County), co-locating Equinix IX, NAPs and the historic MAE-East; **~70% of global internet traffic transits Ashburn** at some point.

**Major IXPs:** Equinix IX (Ashburn/Dallas/NYC/Chicago/LA/Seattle/Miami, ~5 Tbps, 500+ members), NYIIX (NYC, ~5 Tbps), SIX (Seattle non-profit, 300+), Any2/CoreSite (LA), plus US franchises of DE-CIX and AMS-IX.

**Tier-1 backbones:**

| ASN | Carrier | Note |
|---|---|---|
| AS3356 | **Lumen / Level 3** | dominant US backbone |
| AS7018 | AT&T | US + trans-Atlantic |
| AS701 | Verizon (UUNET) | US + both oceans |
| AS174 | Cogent | budget transit, US+Europe |
| AS6461 | Zayo | low-latency US fiber |
| AS6939 | Hurricane Electric | peering-heavy, global |

**Tier-2 (consumer):** Comcast (AS7922, ~32M), Charter/Spectrum (AS20115, ~32M), AT&T consumer (~15M), Verizon Fios, Cox, CenturyLink, T-Mobile/Verizon fixed-wireless.

**CDN/cloud** (all US-HQ, densest PoPs here): Cloudflare (30+ PoPs), Akamai (largest by server count, embedded in ISPs), Fastly, AWS CloudFront (7 US regions), Google, Azure, and **Netflix Open Connect** (appliance embedded in every major ISP). Hyperscaler DC campuses cluster in **Northern Virginia, Silicon Valley, Dallas, Chicago, NY/NJ, Phoenix**, and the cheap-hydro Pacific NW (Google The Dalles, Amazon Boardman, Meta Prineville).

**Cable landings:** Atlantic at Virginia Beach (Marea, Dunant, BRUSA), Tuckerton/Shirley (NJ/NY); Pacific at Hillsboro/Nedonna (OR) and Rancho Palos Verdes/Hermosa/Grover Beach (CA); Hawaii (Maunalua Bay) as the trans-Pacific hub.

**Regulatory/geopolitics:** net-neutrality whiplash (repealed 2017, reinstated 2024, in litigation); **CFIUS / Team Telecom** review of foreign telecom investment and cable licenses (forced abandonment of Google/Meta's HK landing); **CALEA** mandates wiretap capability; **FISA §702** foreign-collection authority; **Huawei/ZTE ban** + $1.9B rip-and-replace; **BEAD** $42.5B rural broadband; **Starlink** (~6,000 sats, ~3M subs, rural + military).

## Canada

Penetration ~94%; regulator **CRTC**. IXPs: **TorIX** (Toronto, ~800 Gbps), VANIX, QIX (Montreal), YYCIX. ISPs dominated by the **Big Three** — Bell (AS577, East), TELUS (AS852, West), Rogers (AS812, which acquired Shaw for C$26B in 2023) — plus Videotron (Quebec) and **SaskTel** (one of the last fully state-owned/provincial-Crown telcos in a developed democracy). Clouds: AWS ca-central-1 (Montreal) + ca-west-1 (Calgary), Azure Central/East; **Quebec's cheap hydropower (~$0.035/kWh)** draws DCs to Montreal. Most international traffic routes via US landings. Regulatory: mandated MVNO wholesale (2021), **Huawei 5G ban** (2022), Bill C-11 streaming act, and Quebec's GDPR-like Law 25.

## Mexico

Penetration ~78%; regulator **IFT**. IXPs in Mexico City (MEXIX/KIO, Uninet IX). The market is defined by **Carlos Slim's América Móvil** — **Telmex/Uninet** (AS8151, ~65% fixed broadband) + Telcel (~70% mobile); IFT declared Telmex a "dominant operator" (2014) under asymmetric regulation. Competitors: Megacable, Totalplay, Izzi (Televisa), Axtel, AT&T Mexico. DCs via Equinix and KIO Networks; **AWS Mexico (Central) region announced 2024**. Cable landings on both coasts (Pacific near Mazatlán, Caribbean near Cancún); ~40% of traffic transits the US via border crossings (San Antonio, El Paso, Tijuana). Notably **has not banned Huawei** despite US pressure.

Related: [[Network Infrastructure]] · [[Latin-America Digital Infrastructure]] · [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
