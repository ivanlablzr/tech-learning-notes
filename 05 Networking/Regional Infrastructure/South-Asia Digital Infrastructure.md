---
type: note
tags: [networking, infrastructure, regional, south-asia]
---


RIR: **APNIC**. Dominated by **India** — which is both the regional infrastructure hub and the world leader in internet shutdowns — with every neighbour except Sri Lanka structurally dependent on Indian transit. (Part of [[Internet & Infrastructure]].)

## India — the hub

Penetration ~65% but **~950M users (world's 2nd-largest internet population)**; regulators **TRAI** (commercial) + **DoT** (security/shutdowns). The market was reshaped by the **Jio effect**: Reliance Jio's 2016 free-data launch crashed mobile data from ~₹250/GB to ~₹7/GB (now the **world's cheapest**), driving mass adoption but consolidating to three players (**Jio** ~460M, **Airtel**, ailing **Vi**; state **BSNL** declining). Plus global transit carrier **Tata Communications**.

- **Peering/cloud:** NIXI exchanges + private Equinix/DE-CIX (~8–9 Tbps aggregate); uniquely deep cloud — **AWS ×2, GCP ×2, Azure ×3** India regions, and Cloudflare/Akamai's densest South-Asian PoPs.
- **Cables:** Mumbai/Chennai land the Europe–Asia corridor (SMW3–6, AAE-1, IMEWE, EIG, BBG); **Reliance Jio is building its own private cables (IAX, IEX)** hyperscaler-style, alongside Google's **Echo**.
- **The shutdown leader:** India performs **more internet shutdowns than any country** (116 in 2023); **Jammu & Kashmir was offline 552 days (2019–21)** after the Article 370 revocation — the longest ever in a democracy. Enabled by the 2017 Telecom Suspension Rules with thin oversight. Also: IT Rules 2021 content takedowns, a fight with WhatsApp over message "traceability," a 2022 VPN-logging mandate (NordVPN/ExpressVPN pulled their India servers), and confirmed **Pegasus** use.

## Pakistan & Bangladesh

- **Pakistan** (~45%, PTA): Jazz (VEON), Telenor, **Zong (China Mobile, 100%)**, state **PTCL** (gateway). It runs national DPI filtering (**PNMS**) and blocks social media during crises (2022–24 Imran Khan protests); the Chinese-funded **PEACE cable** gives it a **non-India-routed** path to Europe — geopolitically significant. Heavy Huawei/ZTE presence.
- **Bangladesh** (~44%, BTRC): Grameenphone (Telenor), Robi (Axiata), Banglalink (VEON), state Teletalk/BTCL. **Cox's Bazar is the *only* cable landing — a single national chokepoint**. The Cyber Security Act criminalizes online speech; during the **2024 anti-quota protests it imposed a 5+ day total shutdown** (to suppress video evidence), and cut the Rohingya camps' mobile internet for 16 months (2019–20). National DC built with Chinese/Huawei help.

## The smaller states — India-dependent

- **Sri Lanka** (~57%): the exception — **7+ cable landings at Colombo** give it **direct connectivity without Indian routing** (can also route via Singapore); SLT-Mobitel (state) + Dialog. Some Chinese presence (Huawei, Hambantota port lease).
- **Nepal** (~62%) & **Bhutan** (~72%): landlocked, **dependent on India** for transit (Bhutan entirely so); Nepal has a politically-sensitive **backup fiber link to China** (Kathmandu–Lhasa). Both relatively open.
- **Maldives** (~82%): island nation on a SMW branch; Dhiraagu/Ooredoo.
- **Afghanistan** (~20%, declining): MTN/AWCC/Salam under **Taliban** ATRA control; US/NATO-funded infrastructure now unmaintained; covert Starlink use.

## The structural picture

International cables land at Mumbai/Chennai → India's T1 ISPs → transit sold onward to Pakistan, Bangladesh, Nepal, Bhutan, Maldives, Afghanistan. **India theoretically controls its neighbours' international connectivity** (un-weaponized, but real leverage); only Sri Lanka is independent. 5G is live only in India (Oct 2022) and tiny Maldives/Sri Lanka pilots — Pakistan/Bangladesh haven't even auctioned spectrum.

Related: [[Internet & Infrastructure]] · [[Southeast-Asia Digital Infrastructure]] · [[East-Asia Digital Infrastructure]] · [[Submarine Cables]] · [[05 Networking|domain overview]]
