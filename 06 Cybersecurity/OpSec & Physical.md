---
type: note
tags: [cybersecurity, physical-security, social-engineering, opsec]
---


The two layers technical controls can't cover: the **physical world** (an attacker with hands on a machine bypasses most logical security) and the **human** (social engineering and operational discipline). The outermost — and often weakest — rings of defense in depth.

## 1. Physical security

Concentric controls, each breached before the next: **perimeter** (fence, bollards, gates) → **building** (doors, mantrap) → **room** (badge+PIN server room) → **rack** (locked cabinet) → **device** (cable lock, BIOS password).

- **Mantrap / sally port** — two interlocking doors (second won't open until the first closes + authenticates), with weight sensors to defeat **tailgating** (unknowing) and **piggybacking** (deliberate).
- **Badge systems** — magstripe and 125 kHz **HID Prox are trivially cloned** (Proxmark within inches); upgrade to 13.56 MHz encrypted cards (iCLASS SE, DESFire) and require **badge + PIN** for sensitive rooms.
- **CCTV** — IP cameras on an isolated VLAN, 30–90 day retention (PCI: 90 min.), tamper/obstruction alerts, regular blind-spot audits.
- **Server-room environment** — 18–27 °C, 40–60 % RH, hot/cold-aisle containment; **clean-agent fire suppression** (FM-200, inert gas IG-55/541; **VESDA** early smoke detection) — Halon is banned.
- **Port & device security** — USB/RJ-45 blockers (defeat BadUSB, rogue media, boot-from-external), plus OS-level USB-storage blocking; laptops get Kensington locks, **FDE**, BIOS passwords, remote wipe (MDM), privacy screens.
- **Faraday/TEMPEST** — conductive shielding blocks EM emanations that leak data (SCIFs, HSMs, faraday bags for seized devices).

Frameworks: ISO 27001 A.11, PCI DSS Req. 9, NIST 800-53 PE family.

## 2. Human-layer security & social engineering

Social engineering targets the **wetware** — the fastest path to initial access in most red-team ops. Every attack pulls **psychological levers**: authority, urgency, scarcity, social proof, reciprocity, liking, familiarity (the best attacks stack several — *urgency + authority + familiarity*).

**Attack vectors:**

| Vector | How it works |
|---|---|
| **Phishing** (email) | mass (fake login pages) → **spear** (OSINT-personalized) → **whaling** (executives) |
| **BEC** | spoof/compromise exec email → instruct finance to wire funds; >$2.7B/yr losses (FBI IC3) |
| **Vishing** (voice) | helpdesk/CEO impersonation; caller-ID spoofing + **AI voice cloning** (30 s of audio) |
| **Smishing** (SMS) | ~98% read rate; fake delivery/bank/2FA lures |
| **Pretexting** | sustained fabricated scenario (IT audit, new hire, vendor, regulator) |
| **Physical** | tailgating with props/uniforms, dumpster diving, shoulder surfing |

**Tooling:** GoPhish (campaigns), **Evilginx2** (reverse-proxy phishing that captures the *session cookie* — defeats TOTP/push MFA but **not FIDO2/WebAuthn**), SET (page cloning), `theHarvester`/hunter.io for OSINT recon. Phishing tells: lookalike/punycode domains, newly-registered domains, display-name ≠ address, urgency, mismatched hover URL.

**Defenses — technical + human:**
- DMARC `p=reject` + SPF/DKIM alignment; anti-phishing gateways (Proofpoint, Mimecast) with URL detonation; **FIDO2 MFA** (the only kind that defeats Evilginx2).
- Continuous, simulated awareness training with a **no-punishment reporting culture** (punishment kills reporting); out-of-band **callback verification** for unusual requests; **two-person authorization** for wire transfers; policy that "caller ID is not authentication."

## 3. Operational security (OPSEC)

Identifying and protecting information that an adversary could piece together against you (a military concept — Operation PURPLE DRAGON). The **5-step process**: identify critical information → analyze threats → analyze vulnerabilities → assess risk → apply countermeasures. *OPSEC is behavior, not just tools — a perfect VPN is worthless if you post your real name on a forum.*

**Match effort to your threat model** (over-OPSEC wastes effort; under-OPSEC leaves gaps): casual stalker → cybercriminal → employer/ISP → law enforcement → nation-state APT.

- **Metadata leakage** — images carry GPS/camera/timestamps; Office/PDF carry author/revision history; email headers carry server IPs. Strip with **ExifTool** (`exiftool -all=`), **MAT2**, or convert via **Dangerzone**.
- **Secure browsing** — harden Firefox (uBlock Origin, etc.) or Brave; **Tor Browser** (3-hop onion; don't resize/extend it or log into personal accounts); **Tails** (amnesic, all-Tor USB OS). Watch for **browser fingerprinting** (canvas/WebGL/fonts) and **DNS leaks** (use DoH/DoT to Quad9/Cloudflare).
- **VPN vs proxy vs Tor** — a VPN shifts trust to the provider (choose verified no-log: Mullvad, ProtonVPN, IVPN; *not* anonymous), a proxy masks one app's IP, **Tor** gives strong anonymity but is slow and the exit node sees unencrypted traffic. `ProxyChains` routes any TCP tool through a proxy chain.
- **Compartmentalization** — separate personas (email/browser-profile/payment per context; SimpleLogin/AnonAddy aliases), burner accounts, separate devices, **Qubes OS** (VM isolation), and **air-gapped** systems for highest-value secrets (bridgeable via BadUSB/acoustic/optical — never plug in untrusted USB).
- **Footprint reduction (OSINT against yourself)** — Google dorking yourself, data-broker opt-outs, HaveIBeenPwned, reverse image search, username enumeration (Sherlock/Maigret), and beware **stylometry** (writing-style de-anonymization).

**Lessons from real OPSEC failures:** Sabu (connected to IRC once without Tor), Ross Ulbricht (real email in early Silk Road posts, PGP keys linked to identity) — a single slip collapses years of discipline.

> **Network surveillance** context: governments, corporations, and criminals monitor internet traffic for control, threat detection, and investigation (CALEA, Total Information Awareness) — the adversary capability OPSEC defends against.

Related: [[06 Cybersecurity|domain overview]] · [[Ethical Hacking#2. The methodology|red-team methodology]] · [[Cybersecurity Foundations#1. What security is|human-layer in the stack]] · [[Information Security & Access#5. Privacy, compliance & governance|privacy]] · [[Endpoint Security#4. Hardware security — the root of trust|hardware security]]
