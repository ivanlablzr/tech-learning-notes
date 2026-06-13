---
type: note
tags: [networking, infrastructure, regional, nordic-baltic]
---


RIR: **RIPE NCC**. The **Nordic-Baltic** region: the world's highest internet/fiber penetration (~97% / ~75% FTTH), the cheapest green electricity in Europe, and a magnet for hyperscale data centers. (Part of [[Network Infrastructure]].)

## The regional backbone

One carrier dominates the spine: **Telia** (AS1299, a Tier-1 backbone; Swedish state ~37%) is the national ISP *and* T1 across Sweden, Finland, Estonia, Latvia, Lithuania, plus Norway. Around it: **Tele2** (Sweden + Baltics), **Telenor** (Norway/Denmark, Norwegian state ~54%), **Elisa** (Finland/Estonia), **Bite** (Baltics).

| Metric | Nordic-Baltic | Global avg |
|---|---|---|
| Internet penetration | ~97% | ~67% |
| FTTH | ~75% | ~35% |
| Renewable-powered DCs | ~90% | ~30% |

## Sweden, Norway, Denmark

- **Sweden** (~97%, PTS): birthplace of **Telia Carrier** (AS1299, a global T1) and **Netnod**, which runs the **I-root DNS server** + Stockholm IXP. ISPs Telia, Tele2, and the privacy-activist **Bahnhof** (hosted WikiLeaks; bunker DC). AWS eu-north-1 (Stockholm); Meta's first EU DC at **Luleå**. Surveillance via the controversial **FRA law** (cable interception).
- **Norway** (~99%, world-leading penetration, Nkom): **Telenor** (state ~54%, operates in 9 countries) + the unique utility-consortium **Altibox** fiber; extraordinary green DCs — **Lefdal Mine** (repurposed mine, fjord-cooled) and **Green Mountain** (underground, 100% hydro).
- **Denmark** (~98%): **TDC** (bought by Macquarie 2018), Telenor, Telia; **Meta hyperscale campuses** at Odense + Viborg, Apple at Viborg; **C-Lion1** cable (Helsinki–Rostock) passes its waters.

## Finland & Iceland — the cooling/power story

- **Finland** (~97%, Traficom): Elisa, Telia/Sonera, DNA (Telenor); **Google's largest European DC at Hamina** (seawater-cooled, europe-north1) + Azure Finland; the strategic **C-Lion1** cable (operated by state-linked Cinia). Nokia's home — collapsed in phones post-iPhone, pivoted to 5G RAN gear competing with Ericsson/Huawei. **NATO member since 2023.**
- **Iceland** (~99%, PFS): 100% renewable geothermal/hydro at ~$0.04/kWh + ~5 °C year-round air → ideal DC site (**Verne Global** on a former NATO base, atNorth, HPC/AI). Cables DANICE, FARICE, IRIS, Greenland Connect; recurrent **Arctic-route** hub proposals.

> **Nordic DC advantage:** cheapest green power in Europe (~$0.03–0.05/kWh) + free natural cooling + political stability + 100%-renewable credentials → Meta (Luleå/Odense/Viborg), Google (Hamina), Apple (Viborg), Microsoft (Espoo) all built here.

## The Baltics — digital governance & front-line geopolitics

- **Estonia** (~97%): the world's most digitally advanced government — **digital ID, X-Road** data-exchange layer (copied in 30+ countries), e-voting. After the **2007 Russian cyberattacks**, it became NATO's cyber hub: **NATO CCDCOE** is in Tallinn. ISPs Telia/Elisa/Tele2.
- **Latvia** (~95%): **Tet** (Latvian state ~51% / Telia ~49%) — a telling case of Swedish-state-linked Telia owning half a Latvian state telco.
- **Lithuania** (~93%): Telia, Bite, Tele2; a vocal Huawei-ban advocate. After blocking Kaliningrad rail transit (2022), Russia threatened to sever Lithuanian fiber (routed through Kaliningrad), prompting rerouting via Latvia/Estonia/Poland.

Related: [[Network Infrastructure]] · [[Western-Europe Digital Infrastructure]] · [[Russia-Cis Digital Infrastructure]] · [[Submarine Cables]] · [[05 Networking|domain overview]]
