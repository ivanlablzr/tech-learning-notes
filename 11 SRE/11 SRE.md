---
type: domain
tags: [domain, moc]
---


> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

Reliability as an engineering discipline: measure it, budget it, and defend it under real failure.

> [!question] The five questions
> **Why it exists:** "it works on my machine" ≠ "it stays up for users"; reliability needs explicit engineering.
> **Problem it solves:** quantifies and protects availability, latency, and durability at scale.
> **Depends on:** [[10 DevOps|DevOps]], [[12 Distributed Systems#Overview|Distributed Systems]].
> **Depended on by:** [[13 Architecture#Overview|Architecture]] (operability as a quality attribute).
> **Interactions:** SLIs/SLOs → error budgets gate release velocity; observability feeds incident response; postmortems feed back into design.

### Sub-areas
- **Reliability concepts** — availability, durability, resilience.
- **SLI/SLO/SLA & error budgets** — the core SRE contract.
- **Observability** (→ bridge) — metrics, logs, traces, OpenTelemetry.
- **Incident management** — on-call, severity, comms, IR.
- **Postmortems & RCA** — blameless learning.
- **Capacity planning** — scaling, forecasting.
- **Chaos / resilience engineering**.

See [[Knowledge Graph#Domain Dependency Graph]].

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[Network Performance & Resilience|Network Performance]], [[Network Performance & Resilience|Network Redundancy]], [[Security Operations & IR|Monitoring and Logging]], [[Security Operations & IR|Network Operations Security]].

## Learning Path

**Prerequisites:** [[10 DevOps|DevOps]] path, [[12 Distributed Systems#Overview|Distributed Systems]] basics.

1. Reliability concepts — availability math, durability, redundancy (N+1/2N).
2. SLIs → SLOs → error budgets — choosing good indicators.
3. Observability — metrics (RED/USE), logs, traces, OpenTelemetry.
4. Alerting — symptom-based, actionable, low-noise.
5. Incident management — on-call, severity, comms.
6. Postmortems & RCA — blameless, action items.
7. Capacity planning & load testing.
8. Chaos engineering — game-days, fault injection.

**Milestones:** instrument a service end-to-end; run a game-day; complete an on-call + postmortem cycle. See [[Learning Paths & Projects#Project Roadmap]].

## Dependencies

**Depends on:** [[10 DevOps|DevOps]] (delivery, observability tooling), [[12 Distributed Systems#Overview|Distributed Systems]] (failure modes), [[00 Mathematics#Overview|Math]] (probability/statistics).

**Depended on by:**
- [[13 Architecture#Overview|Architecture]] — reliability & operability as quality attributes.

```text
DevOps + Distributed Systems → SRE → Architecture
```

## Bridge Topics

Canonical owner of (full notes in [[Knowledge Graph#Bridge Topic Map]]):

- **Observability (SRE ↔ Applications ↔ DevOps)** — *canonical here.* Metrics, logs, traces, and OpenTelemetry to see inside a running distributed system; the difference between monitoring (known) and observability (unknown-unknowns).
- **Probability ↔ SLOs/error budgets** — reuses [[00 Mathematics#Overview|Math]].
- **Load balancing & failover** — links to [[09 Cloud|Bridge Topics]] and [[12 Distributed Systems#Overview]].

## Projects

- **Instrument a service** — RED/USE dashboards, traces, SLOs.
- **Game-day** — inject a failure, observe, recover, measure.
- **On-call simulation** — alert → triage → postmortem.
- Anchors L9 multi-region resilience. See [[Learning Paths & Projects#Project Roadmap]].
