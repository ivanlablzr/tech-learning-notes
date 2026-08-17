---
type: note
tags: [cybersecurity, networking, firewalls, ids]
---


Protecting the network infrastructure itself — controlling who can reach what, detecting intrusions, and architecting for containment so one breach doesn't become total compromise.

## 1. Access control — firewalls & NAC

A **firewall** permits/denies traffic against policy. By increasing capability: **packet-filtering** (stateless, by 5-tuple) → **stateful** (tracks connection state) → **proxy** (terminates & re-originates) → **NGFW** (app-aware, integrated IPS, TLS inspection); a **WAF** protects web apps specifically (detail in [[Switching & Routing#5. Firewalls|Switching & Routing]]). **Default-deny** is the rule: whitelist what's needed, drop everything else.

**Network Access Control (NAC)** gates devices at connection time — checking identity (802.1X), endpoint compliance (patch level, EDR present), and assigning role-based access or quarantine. **ACLs** filter by IP/MAC at switches/routers.

## 2. Segmentation & security zones

Divide the network into isolated zones so compromise of one doesn't grant the others. Tools: **VLANs** (L2), subnets (L3), inter-zone firewalls, ACLs, and **micro-segmentation** (software policy isolating even same-subnet hosts).

| Zone | Trust | Contents |
|---|---|---|
| **Untrusted** | zero | the internet, external partners |
| **DMZ** (semi-trusted) | limited | web servers, mail relay, reverse proxies, VPN gateways, authoritative DNS, API gateways |
| **Trusted (LAN)** | high | workstations, internal app/file servers |
| **Highly trusted** | max | management network, PAWs, OT/SCADA |

**The DMZ** is a buffer holding internet-facing services. The defining rule: **DMZ servers cannot *initiate* connections into the LAN** — so a compromised web server is contained. Preferred architecture is **dual-firewall** (outer filters internet→DMZ, inner tightly controls DMZ→LAN; ideally different vendors for defense in depth) over the cheaper single three-legged firewall. Common mistakes: letting the DMZ reach the LAN, reusing admin credentials across zones, putting the database in the DMZ, and not monitoring DMZ *outbound* traffic (C2).

## 3. Intrusion detection & prevention (IDS/IPS)

The key distinction: **IDS detects and alerts** (passive, out-of-band on a SPAN port, no latency); **IPS detects and blocks** (inline, adds latency, a false positive can cause an outage). By scope: **NIDS/NIPS** watch a network segment (blind to encrypted traffic without TLS inspection); **HIDS/HIPS** run as host agents (see syscalls, file changes, decrypted data — OSSEC, Wazuh, Tripwire, CrowdStrike).

**Detection methods:** **signature** (known patterns — accurate, low FP, but misses zero-days), **anomaly** (baseline deviation — catches novel attacks but noisier), and **reputation** (known-bad IPs/domains/hashes from threat feeds). The core tradeoff is **false negatives** (missed attacks) vs **false positives** (alert fatigue) — tune to minimize FN while keeping FP manageable.

Tuning path: start in detection mode → baseline → whitelist known-good → suppress low-value rules → switch to prevention → monitor for blocked legitimate traffic. Open-source engines **Snort/Suricata** use rule syntax (`action proto src→dst (options)`) and emit structured alerts (Suricata EVE JSON) that should forward to a **SIEM** for correlation (IPS alert + auth failure + DNS = high-confidence chain), enrichment, and retention.

**Evasion** to defend against: fragmentation (reassemble first), slow scans (widen the window), encoding/obfuscation (normalize before matching), encryption (TLS inspection), polymorphism and **living-off-the-land** (behavioral/anomaly detection, not signatures).

## 4. Secure design principles (Saltzer & Schroeder)

| Principle | One line |
|---|---|
| **Defense in depth** | multiple independent layers; one failure isn't fatal |
| **Least privilege** | minimum access needed — limits lateral movement |
| **Separation of duties** | no single entity completes a high-risk action alone |
| **Fail-safe defaults** | default-deny; explicit allow required |
| **Complete mediation** | check *every* access — never cache authorization |
| **Economy of mechanism** | simple > complex (less to audit, fewer bugs) |
| **Open design** | security from strong keys, not secret algorithms (Kerckhoffs) |
| **Psychological acceptability** | usable, or users bypass it (SSO, password managers, push MFA) |
| **Attack-surface minimization** | fewer ports/services/accounts/features = fewer holes |

> Obscurity (e.g. moving SSH to port 2222) reduces scan noise but is **never** a substitute for real controls.

## 5. Modern architectures

- **Zero Trust (ZTA)** — "never trust, always verify": authenticate/authorize every request regardless of network location, using identity + device posture + context; assume breach and minimize blast radius. **ZTNA** replaces VPN broad access with per-app, identity-aware tunnels (SDP: client posture check → controller policy → encrypted gateway).
- **SASE** Architecture (Secure Access Service Edge): the goals is to provide the cloud-native framework which combines network optimization and security of services, enabling secure and efficient access to resources regardless of user location or the location of the application.
	- "**SSE**" (Secure Service Edge):
		- **ZTNA** (Zero Trust Network Access): continuously verifies the *identity* and *application* based *policy enforcement* for controlling access to an organization's sensitive data and applications
		- **SWG** (Secure Web Gateway): provides URL filtering, SSL decryption, application control, and threat detection and prevention for user web sessions
		- **CASB** (Cloud Access Security Broker): oversees sanctioned and unsanctioned SaaS applications and offers malware and threat detection
		- **FWaaS** (Firewall as a Service/Cloud Firewall/Next-generation Firewall): delivers a cloud-native, next-generation firewall, providing advanced Layer 7 inspection, access control, threat detection and prevention, and other security services
	- "**A**" (Network Access)
		- **SD-WAN** (Software-defined wide area network): uses software-defined networking (SDN) principles to manage and optimize *WAN performance*. Today, organizations have too much network traffic to handle because they spread their applications across public clouds, SaaS platforms, hybrid environments, and have to handle remote work and internet traffic. The traditional WAN can't handle that much traffic with just one WAN type connection which will lead to congestion, delays, and higher costs. As more location, users, and apps are added, managing and having the visualization of the networking performance becomes harder.
			- **SD-WAN orchestrator**: provides centralized management, pushing configurations and updates across all sites (orchestrator -> controller)
			- **SD-WAN controller**: continuously monitors the [[Network Performance & Resilience|network performance]] to reroute to the best performing traffic path using it's policy-based traffic management (Control plane = controller -> edge).
			- **SD-WAN edge** (Data plane): it's positioned at the edge of a physical network (branch or cloud sites) to forward traffic based on the local policies instructions given by the controller
			- Types of SD-WAN architecture: On-premises, Cloud-enabled, Cloud-enabled with backbone SD-WAN
			- Benefits: automated policy enforcement, reduced manual work, transport flexibility, cost efficiency, enhanced network and application performance, and improved security (site-to-site encryption, centralize policy controls, SWG, CASB, etc)
- **Remote access:** VPNs/ZTNA, plus mesh overlays (**Tailscale/Netbird** on WireGuard — bypass CGNAT for lab access) and remote-desktop (RDP/VNC/AnyDesk; Sunshine+Moonlight).

## 6. Monitoring & device hardening

**Monitoring:** metrics/logs/traces via SNMP + collectors (Telegraf/Prometheus), dashboards (TIG stack: Telegraf/InfluxDB/Grafana), uptime alerting (Uptime Kuma → Discord/Telegram/Signal), DPI and flow analysis (NetFlow/sFlow), packet capture, and OSI-layer troubleshooting. SIEM (Splunk/Elastic/QRadar) + **SOAR** for correlation and automated response.

**Device hardening:** firmware updates, remove unused services, secure management interfaces, port security + ACLs, strong auth protocols, encrypted management, redundancy (HSRP/VRRP), and physical security. Automate with **Ansible** (config management/IaC across fleets) and **Watchtower** (auto-update containers).

Related: [[06 Cybersecurity|domain overview]] · [[Switching & Routing#5. Firewalls|firewalls]] · [[Internet & Application Security#Zero Trust Architecture|Zero Trust]] · [[Network Types & Topologies#4. VPNs|VPNs]] · [[Security Operations & IR]] · [[System Hardening]] · [[Data Encryption#6. Data in transit|encryption in transit]]
