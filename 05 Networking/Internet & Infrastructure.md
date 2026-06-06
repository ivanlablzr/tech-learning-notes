---
type: note
tags: [networking, internet, bgp, governance, infrastructure]
---


The Internet is **not a cloud — it's a network of networks**: tens of thousands of independently-operated **autonomous systems** that voluntarily interconnect via standardized protocols (TCP/IP), with no single owner. This note covers how those networks find each other (BGP), how they're organized commercially (ISP tiers), where they meet (IXPs), who hands out the numbers (governance), and the physical cables underneath.

## 1. Autonomous Systems & BGP

An **Autonomous System (AS)** is a set of IP prefixes under one administrative entity presenting a single routing policy, identified by a globally-unique **ASN** (16-bit 1–65535, now extended to 32-bit; 64512+ reserved as private). RIRs allocate ASNs.

**BGP (RFC 4271)** is the path-vector protocol that glues the internet together — ASes announce which prefixes they can reach and via what AS-PATH. Key attributes and the (simplified) **path-selection order**:

| Attribute | Meaning |
|---|---|
| **AS-PATH** | list of ASNs traversed (loop prevention + path length) |
| **LOCAL-PREF** | internal outbound preference (higher wins — checked first) |
| **MED** | hint to a neighbor about preferred entry point |
| **Community** | policy tag (e.g. "do not export") |
| iBGP / eBGP | within / between ASes |

Selection: highest LOCAL-PREF → shortest AS-PATH → lowest MED → eBGP over iBGP → lowest IGP cost → lowest router ID. **BGP has no built-in authentication**, so a bad/malicious announcement can hijack global traffic; **RPKI** (cryptographic proof of prefix ownership, managed by RIRs) is the fix, still rolling out.

## 2. ISP tiers & business relationships

Networks arrange into a rough hierarchy by who pays whom:

- **Tier 1** (~14–18 globally — Lumen, AT&T, NTT, Telia, GTT, Cogent): reach the *entire* internet purely through **settlement-free peering** with each other; own long-haul fiber + submarine cables; sell transit downward.
- **Tier 2** (Comcast, Deutsche Telekom, BT, Telstra, Telefónica): buy some transit, peer for the rest; usually own last-mile; most national incumbents.
- **Tier 3** (local/access ISPs): buy all transit, sell to end users.

Many incumbents are wholly or partly **state-owned**, which shapes pricing, censorship, and routing leverage — see [[State-Owned & Government-Linked Carriers]].

**Relationships:** **transit** (customer pays provider for global reachability — ~$0.50–2/Mbps now, down from ~$100 in 2000, billed 95th-percentile), **settlement-free peering** (equal-ish networks exchange free), **paid peering** (a high-traffic sender like a CDN pays an access ISP), and **PNI** (private cross-connect for multi-Tbps relationships).

## 3. Internet Exchange Points (IXPs)

A physical facility (a shared L2 Ethernet fabric + a **route server** for multilateral BGP) where many networks peer directly — "short-circuiting" traffic so a local request never leaves the city or pays transit. Models: cooperative/non-profit (AMS-IX, LINX, DE-CIX), commercial (Equinix IX), neutral, or national. Global IXP traffic grew from ~50 Tbps (2015) to 600+ Tbps (2024).

| IXP | City | Peak |
|---|---|---|
| DE-CIX | Frankfurt | ~14 Tbps |
| AMS-IX | Amsterdam | ~10 Tbps |
| LINX | London | ~8 Tbps |
| JPNAP | Tokyo | ~8 Tbps |
| MSK-IX | Moscow | ~6 Tbps |

## 4. Governance — who hands out the numbers

```
ICANN (policy) → IANA (master registry) → 5 RIRs → LIRs/ISPs → end users
```

- **IANA** (operated by PTI under ICANN) is the single source of truth for three master registries: **IP/ASN pools** (carves `/8` blocks to RIRs), the **DNS root zone** (delegates TLDs, runs DNSSEC root-key ceremonies), and **protocol parameters** (e.g. port 443 = HTTPS). The US relinquished oversight in 2016.
- **RIRs** (ARIN Americas, RIPE NCC Europe/ME/CentralAsia, APNIC Asia-Pacific, LACNIC LatAm, AFRINIC Africa) sub-allocate numbers, run the **WHOIS** database (who owns an IP — used for abuse reports), and manage **RPKI**.
- **IETF** writes the standards (RFCs); **W3C** the web standards; **IEEE** the L1/L2 standards. Governments regulate within borders (data retention, lawful intercept).

## 5. The Internet itself

**History:** ARPANET (1969, packet switching) → TCP/IP (Cerf & Kahn 1974; "Flag Day" cutover Jan 1 1983) → NSFNet backbone (1986, commercial restrictions lifted 1991) → **World Wide Web** (Berners-Lee, CERN, 1991 — an *application* on top of the internet) → broadband → mobile → cloud. ~5.5 billion users (2025).

**A packet's journey:** app → DNS resolves the name → home/edge router → ISP network → cross AS boundaries via BGP peering/transit → maybe an IXP → destination AS → internal routing (OSPF) to the server. Each router makes an independent **longest-prefix-match** decision — connectionless, stateless, no pre-established path.

> **Internet ≠ Web:** the Internet is the infrastructure (routers, cables, TCP/IP/BGP/DNS); the **Web** (HTTP/HTML/URLs) is one application on it, alongside email, SSH, FTP. An **intranet** uses the same tech but private; an **extranet** extends it to trusted partners. **CDNs** ([[Cloud & Datacenters]]) carry 60–80% of traffic by caching at edges and steering users via **anycast** (same IP advertised from many sites → nearest wins).

## 6. Physical layer — submarine cables

~99% of intercontinental traffic rides **submarine fiber cables** (hundreds of systems spanning the transatlantic, transpacific, and SEA-ME-WE corridors). Ownership has shifted from telco consortia toward **hyperscalers** (Google, Meta, Amazon own/co-own a growing share). **Chokepoints** (Red Sea/Suez, Luzon Strait, Strait of Malacca) and shallow **landing stations** are strategic vulnerabilities — a few cable cuts can sever a region (and anchors/sabotage have repeatedly done so). Datacenters and Tier-1 core routers anchor the terrestrial side. Full detail — every major system, route, owner, and the cut/chokepoint risks — is in [[Submarine Cables]].

## 7. Regional infrastructure

The physical internet looks very different region to region — who owns the carriers, where the cables land, which hubs dominate, and how much the state controls it. Each region has its own deep-dive note in **[[Regional Infrastructure/North-America Digital Infrastructure|Regional Infrastructure]]**:

| Region | Defining feature |
|---|---|
| [[North-America Digital Infrastructure]] | **Ashburn** routes ~70% of global traffic; origin of most T1s/CDNs/clouds |
| [[Latin-America Digital Infrastructure]] | a few transnational groups (Claro/Movistar/Tigo); **Brazil's IX.br** is huge |
| [[Western-Europe Digital Infrastructure]] | the **FLAP-D** core (Frankfurt, London, Amsterdam, Paris, Dublin) |
| [[Northern-Europe Digital Infrastructure]] | Nordic green-DC magnet; Telia backbone; Baltic e-governance |
| [[Southern-Europe Digital Infrastructure]] | Mediterranean cable hubs; Balkans owned by a few groups |
| [[Eastern-Europe Digital Infrastructure]] | DT dominance; Ukraine resilience vs Belarus single-gateway |
| [[Russia-Cis Digital Infrastructure]] | state control (SORM/RuNet); post-2022 decoupling |
| [[Middle-East Digital Infrastructure]] | the **Red Sea→Suez→Gulf** chokepoint; state Gulf telcos |
| [[North-Africa Digital Infrastructure]] | **Egypt's Suez chokepoint** (~17% of global traffic) |
| [[Sub-Saharan-Africa Digital Infrastructure]] | cable-led growth (**2Africa**); South Africa the hub |
| [[South-Asia Digital Infrastructure]] | India hub + shutdown leader; neighbours India-dependent |
| [[Southeast-Asia Digital Infrastructure]] | **Singapore** = #2 cable hub & APAC cloud capital |
| [[East-Asia Digital Infrastructure]] | China's **Great Firewall** + $2T splinternet; NTT T1 |
| [[Oceania Digital Infrastructure]] | isolated; Pacific cable subsidy & US–China contest; Starlink |

## 8. Geopolitics

The internet's physical and logical chokepoints are now national-security concerns: **BGP hijacks** (re-routing traffic via false announcements), **submarine-cable sabotage**, **state-owned carriers** as instruments of control and surveillance, **"sovereign internet"** projects (Russia, China) and content filtering, and contests over **DNS-root and number governance** (proposals to move ICANN's role to the UN's ITU). Control of cables, IXPs, and the leading chip/fab supply chain is increasingly strategic.

Related: [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[OSI Layers & Protocols]] · [[Switching & Routing]] · [[Cloud & Datacenters]] · [[Network Types & Topologies]] · [[06 Cybersecurity|network attacks]] · [[05 Networking|domain overview]]
