---
type: domain
tags: [domain, moc]
---

> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

Where every domain converges: making whole-system trade-offs across reliability, security, cost, and evolvability — including **Platform Engineering**, the endpoint of all the chains.

> [!question] The five questions
> **Why it exists:** large systems involve conflicting goals that local decisions can't resolve.
> **Problem it solves:** deliberate trade-off across quality attributes for the whole system and its teams.
> **Depends on:** *all* domains — especially [[12 Distributed Systems#Overview|Distributed Systems]], [[09 Cloud|Cloud]], [[06 Cybersecurity#Overview|Security]], [[11 SRE#Overview|SRE]].
> **Depended on by:** the organization (this is the top of the graph).
> **Interactions:** quality attributes are the axes; patterns are the moves; ADRs record the why.

### Sub-areas
- **Quality attributes / NFRs** — latency, throughput, availability, cost, security, evolvability (the missing axes from [[Analysis & Gaps#Gap Analysis]]).
- **Software architecture** — monolith → microservices → event-driven.
- **Distributed systems core** — CAP, consistency, consensus, discovery.
- **Cloud architecture** — multi-account/region/cloud, hybrid.
- **Security architecture** — Zero Trust, defense in depth.
- **Platform Engineering** — internal developer platforms, golden paths, self-service.
- **Practices** — C4 diagrams, ADRs, capacity/cost modeling.

See [[Knowledge Graph#Domain Dependency Graph]].

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[Network Foundations|Network Architecture]], [[Network Security#Network Security|Secure Network Architecture]], [[Network Security#Network Security|Secure Design Principles]], [[Internet & Application Security#Zero Trust Architecture|Zero Trust Architecture]], [[Programming Foundations|Application - System Architecture]].

## Learning Path

**Prerequisites:** intermediate depth in *all* core domains.

1. Quality attributes / NFRs — name the axes; learn to trade them off.
2. Estimation — back-of-envelope capacity & cost modeling.
3. Software architecture — monolith → microservices → event-driven; when each.
4. Distributed systems core — CAP/consistency/consensus applied to designs.
5. Cloud architecture — multi-account, multi-region, hybrid, multi-cloud.
6. Security architecture — Zero Trust, defense in depth, threat modeling at scale.
7. Resilience patterns — circuit breaker, bulkhead, backpressure.
8. Documentation — C4 model, ADRs.
9. Platform Engineering — IDPs, golden paths, self-service infrastructure.

**Milestones:** write an ADR; design a global SaaS platform with diagrams + cost model (L10). See [[Learning Paths & Projects#Project Roadmap]].

## Dependencies

**Depends on:** essentially everything — [[12 Distributed Systems#Overview|Distributed Systems]], [[09 Cloud|Cloud]], [[06 Cybersecurity#Overview|Security]], [[11 SRE#Overview|SRE]], [[10 DevOps|DevOps]], [[08 Databases#Overview|Databases]], [[05 Networking|Networking]].

**Depended on by:** the organization and its product strategy (top of the graph).

```text
(All domains) → Architecture → Platform Engineering
```

## Bridge Topics

Architecture *is* the meta-bridge — its notes mostly link out to canonical bridges in [[Knowledge Graph#Bridge Topic Map]]:

- **IAM / Zero Trust** — [[09 Cloud|Bridge Topics]] + [[06 Cybersecurity#Bridge Topics]].
- **Consensus / replication** — [[12 Distributed Systems#Bridge Topics]].
- **Observability** — [[11 SRE#Bridge Topics]].
- **Kubernetes / IaC** — [[10 DevOps|Bridge Topics]].
- **Quality attributes** — the lens that ties all bridges to business goals (see [[Analysis & Gaps#Gap Analysis]]).

## Projects

- **ADR practice** — record 3 real trade-off decisions.
- **C4 model a system** — context → container → component.
- **Multi-region resilient system** (L9) — defend RTO/RPO choices.
- **Global SaaS architecture** (L10) — the capstone: diagrams, ADRs, capacity & cost model, thin vertical slice.

See [[Learning Paths & Projects#Project Roadmap]].
