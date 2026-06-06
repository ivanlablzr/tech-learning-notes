---
type: domain
tags: [domain, moc]
---


> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

What changes when one computer becomes many connected by an unreliable network. Promoted to its own domain (it was scattered across Databases/Architecture/Networking — see [[Analysis & Gaps#Gap Analysis]]).

> [!question] The five questions
> **Why it exists:** single machines hit limits of scale, locality, and availability.
> **Problem it solves:** coordinating many nodes to behave like one correct, available system despite partial failure.
> **Depends on:** [[05 Networking|Networking]], [[08 Databases#Overview|Databases]], [[07 Programming|Programming]].
> **Depended on by:** [[09 Cloud|Cloud]], [[11 SRE#Overview|SRE]], [[13 Architecture#Overview|Architecture]].
> **Interactions:** the network's unreliability forces consensus, ordering, and consistency trade-offs that shape every large system.

### Sub-areas
- **Fundamentals** — fallacies of distributed computing, partial failure, FLP.
- **Time & ordering** (→ bridge) — clocks, logical/vector clocks, causality.
- **Consensus** (→ bridge) — Paxos, Raft, quorums, leader election.
- **Consistency models** — linearizable → causal → eventual; CAP/PACELC.
- **Replication & partitioning** — single/multi-leader/leaderless, sharding.
- **Messaging** (→ bridge) — queues, streaming (Kafka), idempotency, exactly-once.
- **Patterns** — sagas, CQRS, event sourcing.

See [[Knowledge Graph#Domain Dependency Graph]].

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[Programming Foundations#Peer-to-Peer (P2P)|Peer-to-Peer (P2P)]], [[Network Performance & Resilience|RAID]], [[Internet & Infrastructure|Autonomous Systems (ASes)]], [[Cloud & Datacenters|Datacenters]].

## Learning Path

**Prerequisites:** [[05 Networking|Networking]], [[08 Databases#Overview|Databases]], [[04 Operating Systems|concurrency]].

1. Fundamentals — the 8 fallacies, partial failure, why distributed is hard.
2. Time & ordering — physical vs logical clocks, happens-before, vector clocks.
3. Consistency models — the spectrum; CAP and PACELC consequences.
4. Replication — leader/follower, multi-leader, leaderless (quorums).
5. Consensus — Raft in depth, then Paxos intuition; leader election.
6. Partitioning/sharding — strategies, rebalancing, hotspots.
7. Messaging — queues vs logs, idempotency, exactly-once illusions.
8. Patterns — saga, CQRS, event sourcing; failure & resilience patterns.

**Milestones:** implement a toy Raft or a quorum KV store; reason through a partition scenario.

## Dependencies

**Depends on:** [[05 Networking|Networking]] (the unreliable channel), [[08 Databases#Overview|Databases]] (state/replication), [[07 Programming|Programming]] (concurrency), [[00 Mathematics#Overview|Math]] (probability, graph theory).

**Depended on by:**
- [[09 Cloud|Cloud]] — managed distributed services.
- [[11 SRE#Overview|SRE]] — failure modes & reliability.
- [[13 Architecture#Overview|Architecture]] — the hard core of system design.

```text
Networking + Databases + Programming → Distributed Systems → (Cloud, SRE, Architecture)
```

## Bridge Topics

Canonical owner of (full notes in [[Knowledge Graph#Bridge Topic Map]]):

- **Consensus (Raft/Paxos)** — *canonical here.* Agree on one value despite failures; powers etcd, ZooKeeper, replicated DBs, [[10 DevOps|Kubernetes]].
- **Message queues / streaming (Kafka)** — *canonical here.* Decouple producers/consumers, absorb load, event sourcing; idempotency & ordering.
- **Time, clocks & ordering (NTP, logical clocks)** — *canonical here.* Agree on before/after without a global clock.
- **Load balancing** — links to [[09 Cloud|Bridge Topics]].

## Projects

- **Toy Raft / quorum KV store** — feel consensus and split-brain.
- **Kafka event pipeline** — producers/consumers, replay, idempotent consumers.
- **Partition simulation** — induce a network partition; observe CAP trade-offs.
- Underpins L7 (K8s) and L9 (multi-region). See [[Learning Paths & Projects#Project Roadmap]].
