---
type: note
tags: [networking, infrastructure, regional, east-asia, china]
---



RIR: **APNIC**. Home to the world's largest user base and its most sophisticated censorship system (China), alongside dense, advanced hubs (Japan, Korea, Taiwan, Hong Kong). The defining feature is the **partial decoupling of the Chinese internet** from the global one. (Part of [[Internet & Infrastructure]].)

## China — a parallel internet

Penetration ~76% but **~1.07 billion users (the world's largest)**; regulators **MIIT** + the more powerful **CAC** (chaired ultimately by Xi via the Central Cyberspace Affairs Commission). The architecture is built for control:

- **Carriers:** three state operators — **China Telecom** (AS4134/4809), **China Unicom**, **China Mobile** — own all backbone; private ISPs merely resell. International traffic is funneled through **~6–8 controlled gateway cities**, and **no neutral IXPs exist** (neutral peering would bypass the inspection points).
- **The Great Firewall (GFW)** — the world's most sophisticated censorship/surveillance system: **DNS poisoning, SNI/DPI inspection, TCP RST injection, BGP blackholing, ML classification** of obfuscated traffic (Shadowsocks/V2Ray), and deniable **throttling**. Blocks Google, Meta, Twitter/X, YouTube, WhatsApp, Wikipedia, most VPNs, and intermittently GitHub. Tightens during "sensitive periods."
- **The splinternet ecosystem:** behind the moat, domestic champions replaced every foreign service — **Baidu** (search), **WeChat/Tencent**, **Weibo**, **Alibaba/Taobao**, **ByteDance**, **DiDi** — a **>$2 trillion** tech sector built on GFW-mediated exclusion. Personal VPN use is illegal-but-tolerated (~14–30% of users); enterprise MPLS VPNs are licensed.
- **Doctrine & leverage:** China promotes "**internet sovereignty**" globally (exported via Huawei/ZTE), enforces **data-localization** (cross-border transfer assessments), and its 5G vendors dominate worldwide — the focus of Western bans.

## Japan — the T1 backbone

Penetration ~95%; regulator **MIC**. **NTT Communications** (AS2914) is a genuine global **Tier-1** (settlement-free with every other T1; spans NA/EU/Asia) atop NTT's ~40M-subscriber Hikari fiber; plus respected ISP **IIJ**. **JPIX/JPNAP** (Tokyo Otemachi, ~3–4 Tbps) anchor peering; Tokyo is a major cable hub (JUPITER, FASTER, Topaz) and hyperscaler region. Open, high-trust internet.

## South Korea & Taiwan

- **South Korea** (~98%, among the world's fastest/most-wired; KCC/MSIT): the chaebol carriers **SK / KT / LG U+**; **KINX** neutral IXP (~1.8 Tbps); strong **domestic platform dominance** — **Naver** (~70% search) and **Kakao** (KakaoTalk ~95%) run their own clouds, preferred for data sovereignty.
- **Taiwan** (~94%, NCC): Chunghwa Telecom incumbent; strategically critical for both **submarine cables** and the **chip/fab supply chain**; faces persistent cross-strait cyber/cable-cut risk (outlying-island cables repeatedly severed).

## Hong Kong, Macau & the isolated states

- **Hong Kong** (~95%, OFCA): a top-tier APAC interconnection hub — **Equinix HK1–6**, SUNeVision MEGA (its largest DC), no GFW. But post-**National Security Law (2020)** the trajectory is uncertain: technically open, yet self-censorship is rising and the FCC blocked the **Pacific Light** cable's HK landing on security grounds. **Macau** mirrors HK under "One Country, Two Systems."
- **Mongolia** (~72%): independent, routes via Russia/China.
- **North Korea**: essentially no internet — **~5 BGP prefixes / ~30 routable IPs** for the whole country; elites only, the public on the **Kwangmyong** intranet; upstream via China Unicom + Rostelecom (see [[State-Owned & Government-Linked Carriers]]).

> **Systemic risk:** with 1B+ users behind the GFW and China promoting an alternative governance model, a deeper China decoupling is the single biggest fragmentation risk to the global internet.

Related: [[Internet & Infrastructure]] · [[Southeast-Asia Digital Infrastructure]] · [[Russia-Cis Digital Infrastructure]] · [[Submarine Cables]] · [[State-Owned & Government-Linked Carriers]] · [[05 Networking|domain overview]]
