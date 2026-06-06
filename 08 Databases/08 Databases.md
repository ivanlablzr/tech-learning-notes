---
type: domain
tags: [domain, moc]
---


> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

How systems store, index, and reliably query state. A P0 domain dropped from the source build-first list (see [[Analysis & Gaps#Gap Analysis]]).

> [!question] The five questions
> **Why it exists:** programs need durable, queryable, consistent state beyond memory.
> **Problem it solves:** persistence, efficient retrieval (indexes), and correctness under concurrency (transactions).
> **Depends on:** [[07 Programming|Programming]], [[04 Operating Systems|OS]] (storage, concurrency).
> **Depended on by:** [[12 Distributed Systems#Overview|Distributed Systems]], [[14 AI|AI]], nearly every application.
> **Interactions:** transactions (ACID) meet [[12 Distributed Systems#Overview|distributed]] reality (CAP); indexes are algorithms on disk.

### Sub-areas
- **Relational** — model, SQL, normalization (PostgreSQL, MySQL).
- **Database internals** — storage engines, indexes (B-tree/LSM), query planner, transactions, WAL, MVCC.
- **NoSQL** — key-value, document, column, graph (DynamoDB, Mongo, Cassandra).
- **Distributed data** — replication, sharding, consensus (→ [[12 Distributed Systems#Overview]]).
- **Caching** — read-through, write-back, invalidation.

See [[Knowledge Graph#Domain Dependency Graph]].

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[Network Foundations|Data]], [[Data Encryption|Database Encryption]], [[Data Encryption|Transparent Data Encryption (TDE)]], [[Languages & Applied Programming|Web Development]], [[Storage#Storage|Storage]].

## Learning Path

**Prerequisites:** [[07 Programming|Programming]] (data structures), [[04 Operating Systems|OS]] (files, concurrency).

1. Relational model & SQL — joins, aggregation, normalization.
2. Indexes — B-trees vs LSM-trees; query plans (EXPLAIN).
3. Transactions — ACID, isolation levels, MVCC, WAL.
4. Concurrency control — locks, deadlocks (reuses OS concurrency).
5. NoSQL families — when and why each.
6. Replication & sharding — single/multi-leader/leaderless.
7. Distributed data — CAP/consistency (handoff to [[12 Distributed Systems#Overview]]).
8. Caching strategies & invalidation.

**Milestones:** design a schema + tune a slow query with an index; reason about an isolation anomaly.

## Dependencies

**Depends on:** [[07 Programming|Programming]], [[04 Operating Systems|OS]] (storage/concurrency), [[00 Mathematics#Overview|Math]] (relational algebra, sets).

**Depended on by:**
- [[12 Distributed Systems#Overview|Distributed Systems]] — replicated/sharded data.
- [[14 AI|AI]] — training data, vector stores.
- [[13 Architecture#Overview|Architecture]] — data is usually the hardest part to scale.

```text
Programming + OS → Databases → (Distributed Systems, AI, Architecture)
```

## Bridge Topics

- **Consensus (Raft/Paxos)** — replicated databases need agreement; canonical owner is [[12 Distributed Systems#Bridge Topics]].
- **Transactions ↔ distributed transactions** — ACID meets partitions (CAP). Links to [[12 Distributed Systems#Overview]].
- **Indexes as algorithms** — B-tree/LSM connect [[07 Programming|algorithms]] to disk.
- **Caching ↔ load** — connects to [[09 Cloud|load balancing]] and CDN.

See [[Knowledge Graph#Bridge Topic Map]].

## Projects

- **Schema + query tuning** — model a domain, add indexes, read query plans.
- **Build a tiny KV store** with a WAL — feel durability.
- **Replication experiment** — set up primary/replica, induce lag/failover.
- Supports L7–L9 (stateful services in K8s, multi-region data). See [[Learning Paths & Projects#Project Roadmap]].
