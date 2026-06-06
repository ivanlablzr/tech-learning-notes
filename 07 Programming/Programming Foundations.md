---
type: note
tags: [programming, architecture, fundamentals]
---


How software is structured and how to write it well — the architectural and discipline concepts beneath any language.

## System & software architecture

The first question of any system is **"who owns the data and has authority?"**

- **Centralized** — a server is the authority (server-to-server, **server-to-client**). Simple to manage and secure; the server is a single point of failure and a scaling bottleneck.
- **Decentralized / distributed** — authority is spread out. **Client-to-client** still often needs a server to introduce peers (e.g. messaging). **Peer-to-peer (P2P)** has no central authority — every node is both client and server.

**Application architecture** evolves from **monolith** (UI + business logic + data access in one deployable) to **microservices** (loosely-coupled, independently-deployable services, each owning one business capability, communicating over APIs, usually containerized). Microservices buy scalability and independent deployment at the cost of operational and distributed-systems complexity — Netflix, Amazon, and Uber all made this shift as they grew. Deep dive: [[12 Distributed Systems]], [[13 Architecture]].

## Peer-to-peer (P2P)

Symmetric nodes communicate directly. Discovery is by broadcast (LAN), **DHT** (BitTorrent/Kademlia), or bootstrap nodes; **hybrid** P2P uses a central tracker for discovery but transfers peer-to-peer; **federated** servers interoperate (ActivityPub/Mastodon, Matrix, email).

| | P2P | Client-server |
|---|---|---|
| Scalability | high (more peers = more capacity) | server-capped |
| Single point of failure | none | the server |
| Admin / security | harder (no central trust) | easier (central policy) |

**Uses:** file sharing (BitTorrent), cryptocurrency (Bitcoin/Ethereum), anonymity (Tor/I2P), real-time media (WebRTC), distributed compute (Folding@home), content addressing (IPFS). **Threats:** malware distribution, **Sybil attacks** (many fake identities), routing/DHT poisoning, traffic analysis.

## Writing effective code — NASA's "Power of 10"

NASA/JPL's safety-critical coding rules generalize to robust software anywhere:

1. Keep control flow simple — no `goto`/recursion that obscures flow.
2. **Bound every loop** (no unbounded/infinite loops).
3. Avoid dynamic memory after init (predictable memory — no heap surprises).
4. Keep **functions short** (~≤60 lines — one screen, one job).
5. Use **assertions** liberally (≥2 per function) to check assumptions.
6. Declare data at the smallest possible scope (easier to reason about and debug).
7. Check every return value and validate every input.
8. Limit the preprocessor (source > macros).
9. Restrict pointers/indirection.
10. **Compile with all warnings on**, zero warnings, and static-analyze.

The underlying goals — **security, functionality, verifiability, readability** — are what separate code that merely runs from code that can be trusted and maintained.

Related: [[Languages & Applied Programming]] · [[07 Programming|domain overview]] · [[12 Distributed Systems]] · [[13 Architecture]] · [[06 Cybersecurity|secure coding]]
