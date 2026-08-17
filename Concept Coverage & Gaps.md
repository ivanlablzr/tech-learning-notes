---
role: roadmap
tags: [meta, backlog, gaps]
domains: [all]
---

> [!abstract] The planning layer for the [[Master Index — Technology Vault|Concept Atlas]] — what's covered, what's thin, and what to write next. The Atlas *describes* the vault; this note *tracks* it. Kept separate so the index stays purely descriptive.

## Coverage matrix — depth by domain
`✓✓` deep (★ atomic notes) · `✓` overview · `◐` partial · `⛏` thin/gap.

| # | Domain | Shape | Depth | Biggest gaps |
|---|---|---|---|---|
| 00 | Mathematics | fan-out | ⛏ | linear algebra, probability, number theory |
| 01 | Physics | limits ladder | ⛏ | semiconductor & quantum physics |
| 02 | Electronics | abstraction lift | ✓✓ | sequential logic, ALU |
| 03 | Hardware | data hierarchy | ✓✓ | caches, pipelining, side-channels ⚠ |
| 04 | Operating Systems | OSTEP triad | ✓✓ | concurrency primitives, scheduling |
| 05 | Networking | encapsulation ladder | ✓✓ | BGP/routing, DHCP, load balancing |
| 06 | Cybersecurity | attack⟷defense mirror | ✓✓ | threat modeling, detection eng, supply-chain ⚠ |
| 07 | Programming | ladder of concerns | ◐ | DS&A internals, compilers, concurrency |
| 08 | Databases | query→disk / CAP triangle | ◐ | SQL/indexing internals, replication |
| 09 | Cloud | responsibility ladder | ◐ | IAM ⚠, multi-tenancy, serverless |
| 10 | DevOps | delivery loop | ⛏ | CI/CD, Kubernetes, secrets ⚠ |
| 11 | SRE | control loop | ◐ | SLOs, incident response |
| 12 | Distributed | impossibility web | ✓✓ | CAP, idempotency, sagas |
| 13 | Architecture | quality-attribute space | ⛏ | service boundaries, patterns |
| 14 | AI | pipeline × capability | ◐ | transformers/attention, MLOps |
| 15 | Quantum | classical-vs-quantum mirror | ⛏ | post-quantum crypto ⚠ |
| 17 | Tech Economy | value chain | ✓ | — |
| 18 | Robotics/Bio | sense→think→act loop | ◐ | control systems, BCI |

## Gap list — priority to-write
Ranked by edge-degree (how many notes already point at them) and cross-cutting value:

1. **Threat Modeling** (06) ⚠
2. **Idempotency** (12 / glue)
3. **Caching & Invalidation** (glue)
4. **Naming & Addressing** (glue)
5. **CPU caches / pipelining / speculative execution** (03) ⚠
6. **Concurrency primitives** — locks, deadlock, races (04/07) ⚠
7. **Detection Engineering** (06)
8. **Supply-Chain Security** (06/10) ⚠
9. **Backpressure & Flow Control** (glue)
10. **SQL & indexing internals** (08)
11. **Kubernetes & orchestration** (10)
12. **Transformers & attention** (14)
13. **IAM & cloud multi-tenancy** (09) ⚠
14. **Post-Quantum Cryptography** (15 / glue) ⚠

## Related
[[Master Index — Technology Vault]]
