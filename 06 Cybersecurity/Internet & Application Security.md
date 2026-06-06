---
type: note
tags: [cybersecurity, appsec, websec, zerotrust]
---


Securing software and the services exposed on the public internet — the application layer, the browser security model, email/DNS authentication, and the modern identity-centric architecture (Zero Trust) that replaces the old perimeter.

## Application Security

Build security into the **SDLC**, don't bolt it on. Core practices: **input validation** (never trust user data), **output encoding** (context-aware, stops XSS), **secure error handling** (no stack traces to users), **parameterized queries**, and dependency/secret hygiene. Testing layers: **SAST** (static — scans source), **DAST** (dynamic — attacks the running app), **IAST** (instrumented hybrid), **SCA** (dependency CVEs), plus **threat modeling** (STRIDE/PASTA) at design time. Runtime controls: **WAF** (filters HTTP attacks), **RASP** (in-app self-protection).

## OWASP Top 10

The standard list of critical web-app risks:

| # | Risk | Key mitigation |
|---|---|---|
| **A01** | **Broken Access Control** (IDOR, privilege escalation) | server-side authz on *every* request; default-deny; UUIDs not sequential IDs |
| **A02** | **Cryptographic Failures** (cleartext, weak hashing) | TLS everywhere + HSTS; Argon2/bcrypt for passwords; AES-GCM at rest |
| **A03** | **Injection** (SQLi, NoSQLi, command, LDAP) | parameterized queries; never `shell=True`; strict input validation |
| **A04** | **Insecure Design** (logic flaws) | threat modeling; security requirements; OWASP ASVS |
| **A05** | **Security Misconfiguration** (open S3, debug endpoints, default creds) | hardened minimal configs; config scanning (ScoutSuite, Trivy) |
| **A06** | **Vulnerable Components** (Log4Shell, Struts) | SBOM + SCA (Snyk, Dependabot); patch SLAs |
| **A07** | **Auth Failures** (weak sessions, brute force) | MFA; ≥128-bit random tokens; secure cookies; rate limiting |
| **A08** | **Data Integrity Failures** (insecure deserialization, supply chain) | sign & verify artifacts; no untrusted deserialization; secure CI/CD |
| **A09** | **Logging & Monitoring Failures** | log auth/authz events to SIEM; alert on anomalies; off-host storage |
| **A10** | **SSRF** | allowlist URLs; block RFC1918 + 169.254.0.0/16; AWS IMDSv2 |

> The two most impactful classes: **injection** (untrusted data hits an interpreter — fixed by parameterization) and **broken access control** (missing server-side ownership/role checks). A WAF reduces risk but never replaces [[System Hardening|secure coding]].

## Internet Security

**Threat landscape** by actor: cybercriminals (ransomware, BEC, fraud), nation-states/APTs (espionage, supply chain, zero-days), hacktivists (DDoS, defacement), insiders, and botnets. Common delivery: **phishing** (spear/whaling/vishing/smishing), **watering hole** (compromise a site the target trusts), **drive-by download** (auto-exploit on visit), and **supply chain** (SolarWinds 2020, XZ Utils 2024 backdoor, npm/PyPI typosquatting).

**Browser security model** — the layered defenses servers send via headers:

| Mechanism | Protects against |
|---|---|
| **Same-Origin Policy** | cross-site reading of your session/cookies (the foundation) |
| **CORS** | controlled SOP exceptions (`*` + credentials is a classic bug) |
| **CSP** | XSS — restricts which script/style/resource sources load |
| **HSTS** (`+preload`) | SSL stripping — forces HTTPS even on first visit |
| **Certificate Transparency** | mis-issued certificates (all certs publicly logged) |
| `X-Frame-Options` / `X-Content-Type-Options` / `Referrer-Policy` / `Permissions-Policy` | clickjacking, MIME sniffing, referrer leakage, API abuse |

Browsers also sandbox each tab in its own process (sandbox escapes are high-value zero-days) and trust extensions with full page access (install only vetted ones).

**Email authentication** — all three needed, correctly configured:
- **SPF** (DNS TXT lists authorized sending IPs; `-all` = hard fail).
- **DKIM** (signs headers/body; recipient verifies via DNS public key).
- **DMARC** (ties SPF+DKIM to a policy; only **`p=reject`** actually blocks spoofing — `p=none` just monitors).

**DNS security:** **DNSSEC** signs records (kills cache poisoning; integrity not confidentiality); **DoH/DoT** encrypt queries (DoH over 443 is indistinguishable from HTTPS); **DNS filtering** (Pi-hole, Cisco Umbrella, RPZ) blocks malicious domains.

**DDoS protection:** scrubbing centers (Cloudflare Magic Transit, Akamai, AWS Shield) absorb volumetric (L3/4) attacks via anycast; TCP SYN cookies handle protocol floods; WAF + rate limiting + CAPTCHA handle L7; RTBH blackholing is the last resort (detail in [[Threats & Malware#3. DoS / DDoS — deeper|Threats & Malware]]).

## Zero Trust Architecture

**"Never trust, always verify"** — the perimeter is gone (cloud, remote work, SaaS, BYOD), so trust is never granted by network location. Every request is authenticated, authorized, and continuously re-evaluated; the design **assumes breach** and grants **least privilege**. Standardized in **NIST SP 800-207**; Google's **BeyondCorp** (post-Operation-Aurora, 2011) was the pioneering implementation — access by *who you are + what device*, through an access proxy, with no privileged internal network.

Six pillars: **Identity** (MFA, phishing-resistant FIDO2 for privileged, federated SSO, JIT elevation) · **Device** (posture/health attestation, compliance via Intune/Jamf/CrowdStrike, machine certs) · **Network** (microsegmentation, SDP, encrypted east-west) · **Workload** (mTLS between services, workload identity, CASB) · **Data** (classification, encryption, DLP, IRM) · **Visibility** (continuous logging, SIEM, UEBA feeding the policy engine).

**Microsegmentation** applies per-workload firewall rules so even same-segment hosts can't talk unless explicitly allowed — kills lateral movement (Illumio, VMware NSX, Kubernetes NetworkPolicies). **Risk-based adaptive access** steps up auth as signals worsen (new country / suspicious behavior → re-auth or block; Entra Conditional Access).

**ZTNA vs VPN:** a VPN grants broad network trust once connected; **ZTNA** grants per-app access evaluated each request — no lateral movement, continuous device checks, agentless third-party access, and direct-to-SaaS performance (Zscaler ZPA, **SASE/SSE** delivering ZTNA+SWG+CASB+FWaaS from the cloud edge).

## IoT Security

Billions of devices, disproportionately insecure. **Attack surface:** default/hardcoded credentials (identical across a model, found by Shodan in hours), cleartext comms (MQTT/HTTP/Telnet), no update mechanism, exposed UART/JTAG, and tight resource constraints. **Mirai (2016)** weaponized this — scanned Telnet, tried ~60 default creds, built a ~600K-device botnet, and hit Dyn with ~1.2 Tbps, taking down much of the US internet.

**Secure design (OWASP IoT Top 10):** no default passwords (set at setup, tied to a per-device secret — now mandated by UK PSTI / California SB-327), **signed firmware** with anti-rollback (ECDSA, key in write-protected ROM), encrypted comms (lightweight TLS — mbedTLS/WolfSSL/BearSSL, or TLS-PSK), minimal attack surface (disable debug/unused services), and **MQTT hardening** (TLS on 8883, auth, per-topic ACLs). Operationally: put IoT on **its own VLAN** (block IoT→corporate), 802.1X + NAC, DNS filtering, and manage fleets via AWS IoT Core / Azure IoT Hub with OTA updates and zero-touch certificate provisioning.

## AI and Machine Learning Security

ML adds a new attack surface, both *against* models and *by* adversaries:

- **Adversarial examples** — imperceptible gradient-computed perturbations cause misclassification (panda→gibbon; stop-sign stickers; malware evasion). White-box (full access), black-box (query-based, transferable), or physical. Defenses: adversarial training, input preprocessing, randomized smoothing, ensembles.
- **Model inversion / extraction** — reconstruct training data or clone the model via queries. Defenses: differential privacy (DP-SGD), return top-class only, rate-limit/monitor queries, watermarking.
- **Data poisoning / backdoors** — contaminate training data so a trigger causes misbehavior (esp. federated learning, scraped/open datasets). Defenses: data provenance, outlier detection, robust aggregation (Krum, Trimmed Mean).
- **Prompt injection** (LLM apps) — user/embedded content overrides system instructions (direct: "ignore previous instructions"; indirect: hidden text in a fetched page). Defenses: privilege separation of system vs user content, output validation, least-privilege tools + human-in-the-loop, injection classifiers.
- **AI-generated threats** — scaled multilingual spear-phishing, voice-cloned vishing, and **deepfakes** authorizing fraudulent transfers (documented $25M+ cases). Defense: out-of-band verification for high-stakes actions.

Defensively, ML powers anomaly detection (network, **UEBA**, host), threat-intel automation, and SOC triage (Darktrace, Vectra, Exabeam, Microsoft Copilot for Security). Secure ML dev: data governance, model cards, access control, versioning/rollback, adversarial red-teaming, and supply-chain checks on pre-trained models.

## Cloud Security

Governed by the **shared responsibility model** (provider secures the cloud; customer secures what's *in* it). Top risks: **misconfiguration** (public S3 buckets, over-permissive IAM roles), insecure APIs, and data leakage. Controls map to: **manage access** (IAM, least privilege, network policy), **gain insight** (CSPM posture, compliance, threat detection — GuardDuty), and **protect data** (encryption at rest/in transit, **BYOK/KYOK** key management, container & serverless security). Delivered at the edge via **SASE/SSE** (CASB, SWG, ZTNA, FWaaS) — see [[09 Cloud]].

Related: [[06 Cybersecurity|domain overview]] · [[Threats & Malware#1. Malware taxonomy|threats & malware]] · [[Network Security]] · [[System Hardening]] · [[Data Encryption#6. Data in transit|TLS]] · [[OSI Layers & Protocols#Domain Name System (DNS)|DNS]] · [[Information Security & Access#1. Identity & Access Management (IAM)|IAM]] · [[14 AI]]
