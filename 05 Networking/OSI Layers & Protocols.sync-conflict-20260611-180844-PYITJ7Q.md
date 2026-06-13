---
type: note
tags: [networking, osi, protocols, dns, http]
---


The **OSI model** is the universal framework for networking — every protocol, attack, and defense maps to one of its seven layers, turning networking from scattered facts into a coherent system. ISO created it (1984) for vendor interoperability; today it's the **teaching and troubleshooting** lens, while the real internet runs **TCP/IP**.

> Mnemonic (L7→L1): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

## The seven layers

| L | Layer | PDU | Function | Key protocols | Signature attacks → defense |
|---|---|---|---|---|---|
| 7 | **Application** | data | app-facing services | HTTP/S, DNS, DHCP, SMTP/IMAP, SSH, SNMP, NTP | SQLi/XSS/CSRF, phishing, API abuse → WAF, input validation, auth |
| 6 | **Presentation** | data | encoding, encryption, compression | **TLS/SSL**, UTF-8, JSON/XML/protobuf, gzip | SSL stripping, encoding attacks → HSTS, cert validation |
| 5 | **Session** | data | open/maintain/close dialogues | NetBIOS, RPC, **SIP**, NFS | session hijacking/fixation → secure random tokens, timeouts |
| 4 | **Transport** | segment/datagram | end-to-end delivery, **ports** | **TCP, UDP**, QUIC | SYN flood, UDP amplification, port scan → SYN cookies, rate limit |
| 3 | **Network** | packet | logical addressing + routing | **IPv4/IPv6, ICMP**, OSPF, **BGP**, IPsec, NAT | IP spoofing, BGP hijack, ICMP redirect → RPKI, uRPF, ACLs |
| 2 | **Data-Link** | frame | frames on one segment, MAC | Ethernet (802.3), Wi-Fi (802.11), **ARP**, STP, 802.1Q | ARP poisoning, MAC flooding, VLAN hopping → DAI, port security, DHCP snooping |
| 1 | **Physical** | bits | signals on the medium | cables/fiber/radio, RJ-45, OFDM | tapping, jamming, rogue AP, implants → physical security, 802.1X |

**Encapsulation:** data travels *down* the stack, each layer adding a header (and the data-link a trailer/FCS) → bits on the wire; the receiver **decapsulates** *up*, stripping each header. **Sublayers** of L2: **LLC** (interface to L3) + **MAC** (addressing + media access — CSMA/CD for Ethernet, CSMA/CA for Wi-Fi).

**OSI vs TCP/IP** (what's actually implemented):

| TCP/IP | OSI | Protocols |
|---|---|---|
| Application | 5–7 | HTTP, DNS, SSH, TLS |
| Transport | 4 | TCP, UDP |
| Internet | 3 | IP, ICMP, IPsec |
| Network Access | 1–2 | Ethernet, Wi-Fi, ARP |

> Knowing *which layer* an attack targets tells you *where* to apply the defense.

## Transport layer in depth (L4)

**Ports** identify which app receives data: **well-known** 0–1023 (need root to bind), **registered** 1024–49151, **ephemeral** 49152–65535 (client source). Common: 80 HTTP, 443 HTTPS, 22 SSH, 53 DNS, 25 SMTP, 3389 RDP, 445 SMB.

| | **TCP** | **UDP** |
|---|---|---|
| Connection | 3-way handshake (SYN→SYN-ACK→ACK) | connectionless |
| Reliability | ACKs, retransmit, ordering (seq #) | none — app's problem |
| Control | flow (sliding window) + congestion (AIMD, slow start) | none |
| Use | HTTP, SSH, FTP, SMTP | DNS, DHCP, VoIP, streaming, **QUIC/HTTP-3** |

## Domain Name System (DNS)

The internet's phone book — translates names (`google.com`) to IPs. **Resolution chain:** a **recursive resolver** (your ISP / 8.8.8.8 / 1.1.1.1) queries the **root** (`.`) → **TLD** server (`.com`, run by Verisign) → **authoritative** server (the domain's own nameservers) → returns the record (then caches it per **TTL**).

**Record types:** **A**/**AAAA** (IPv4/IPv6), **CNAME** (alias), **MX** (mail), **NS** (nameservers), **TXT** (SPF/DKIM/verification), **SOA** (zone authority), **PTR** (reverse). 
**Governance:** ICANN/IANA manage the root zone; **13 root server identities** (A–M) run on hundreds of **anycast** instances worldwide (F-root alone 300+). Root governance is geopolitically contested (proposals to move it to the UN's ITU).

**DNS security:** **cache poisoning** (forged responses) and **DNS tunneling** (exfiltration over DNS) → mitigated by **DNSSEC** (signed records) and **DoH/DoT** (DNS over HTTPS/TLS — encrypt queries).

## HTTP

The web's application protocol (request → response over TCP, or QUIC for HTTP/3).

- **Methods:** GET (read, idempotent/cacheable), POST (create), PUT (replace), PATCH (update), DELETE, OPTIONS (CORS pre-flight).
- **Status codes:** 2xx success (200 OK, 201 Created), 3xx redirect (301/302, 304 Not Modified), 4xx client (400/401/403/404, 429 Too Many Requests), 5xx server (500, 502, 503).
- **Versions:** HTTP/1.1 (text, one request at a time per connection) → **HTTP/2** (binary, multiplexed streams, header compression) → **HTTP/3** (over **QUIC/UDP** — no TCP head-of-line blocking, 0-RTT). **HTTPS** = HTTP over TLS. Web-app security in [[Internet & Application Security]].

Related: [[Network Foundations]] · [[Switching & Routing]] · [[Network Devices]] · [[Network Infrastructure]] · [[06 Cybersecurity|attacks by layer]] · [[05 Networking|domain overview]]
