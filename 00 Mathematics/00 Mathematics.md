---
type: domain
tags: [domain, moc]
aliases: [Math]
---


> [!info] Domain note — five views below: Overview · Learning Path · Dependencies · Bridge Topics · Projects.


## Overview

The substrate beneath all of computing. Most IT learners underestimate it; cryptography, ML, networking, and complexity all dissolve into hand-waving without it.

> [!question] The five questions
> **Why it exists:** to reason precisely about quantity, structure, change, and uncertainty.
> **Problem it solves:** gives exact, provable foundations for representation (binary), logic (gates), security (number theory), and learning (linear algebra/stats).
> **Depends on:** nothing inside computing — it is a root.
> **Depended on by:** [[01 Physics#Overview|Physics]], [[07 Programming|Programming]], [[06 Cybersecurity#Overview|Cryptography]], [[14 AI|AI]], [[12 Distributed Systems#Overview|Distributed Systems]].
> **Interactions:** number bases → hardware; boolean algebra → logic gates; modular arithmetic → RSA/ECC; linear algebra → neural nets; probability → ML & reliability.

### Sub-areas
- **Number systems & arithmetic** — binary, hex, octal, two's complement.
- **Boolean algebra & logic** — the bridge to [[02 Electronics|logic gates]].
- **Discrete math** — sets, relations, graph theory (routing, dependency graphs).
- **Number theory** — modular arithmetic, primes → cryptography.
- **Linear algebra** — vectors/matrices → graphics, ML.
- **Probability & statistics** — distributions, hypothesis testing → ML, SRE, capacity planning.
- **Information theory** — entropy, channel capacity → compression, error correction, crypto.

See [[Knowledge Graph#Domain Dependency Graph]] and [[Analysis & Gaps#Gap Analysis]] (Mathematics was a P0 missing root).

### Related notes in this vault

Existing notes this domain connects to (wired into the knowledge graph):

[[Hardware Logic & Fabrication|Computational Logic]], [[Network Foundations|Data]], [[Digital Logic & Microcontrollers|Digital Electronics]].

## Learning Path

**Prerequisites:** none (this is a root domain).

1. Number bases & computer arithmetic — binary/hex/octal, two's complement, IEEE-754.
2. Boolean algebra & propositional logic → feeds [[02 Electronics|logic gates]].
3. Discrete math — sets, relations, functions, graph theory.
4. Combinatorics & basic complexity (Big-O intuition) → feeds [[07 Programming|algorithms]].
5. Probability & statistics — distributions, Bayes, hypothesis testing.
6. Linear algebra — vectors, matrices, eigenvalues → feeds [[14 AI|ML]].
7. Number theory & modular arithmetic → feeds [[06 Cybersecurity#Learning Path|cryptography]].
8. Information theory — entropy, Shannon, channel capacity.

**Milestones:** truth-table → gate mapping (supports L0 [[Learning Paths & Projects#Project Roadmap|Logic-to-CPU]]); implement modular exponentiation by hand (supports PKI lab).

## Dependencies

**Depends on:** nothing within computing (root).

**Depended on by:**
- [[01 Physics#Overview|Physics]] — calculus, vectors for fields/waves.
- [[02 Electronics|Electronics]] — boolean algebra → logic gates.
- [[03 Computer Hardware#Overview|Hardware]] — computer arithmetic, two's complement, floating point.
- [[07 Programming|Programming]] — complexity, discrete structures, algorithms.
- [[06 Cybersecurity#Overview|Cryptography]] — number theory, modular arithmetic.
- [[14 AI|AI]] — linear algebra, probability, optimization.
- [[12 Distributed Systems#Overview|Distributed Systems]] — graph theory, probability, logical ordering.
- [[05 Networking|Networking]] — information theory (channel capacity, coding).

```text
Mathematics → (Physics, Programming, Crypto, AI, Distributed, Networking)
```

## Bridge Topics

Math itself is the ultimate bridge. The connectors where it meets other domains (canonical notes live in [[Knowledge Graph#Bridge Topic Map]]):

- **Boolean algebra ↔ Logic gates** — math becomes hardware. Owner: [[02 Electronics|Bridge Topics]].
- **Modular arithmetic ↔ Public-key crypto** — math becomes [[06 Cybersecurity#Bridge Topics|TLS/PKI]].
- **Linear algebra ↔ Neural networks** — math becomes [[14 AI|GPU/CUDA]] workloads.
- **Graph theory ↔ Routing & dependency graphs** — math becomes [[05 Networking|networks]].
- **Probability ↔ Reliability/SLOs** — math becomes [[11 SRE#Overview|error budgets]].
- **Information theory ↔ Compression & coding** — math becomes channel design.

## Projects

Math is proven by *use* inside other projects rather than standalone builds.

- **Truth-table → gate design** — supports L0 [[Learning Paths & Projects#Project Roadmap|Logic-to-CPU simulator]].
- **Implement RSA from scratch** (modexp, key gen) — supports L3 [[Learning Paths & Projects#Project Roadmap|PKI lab]].
- **Subnet calculator** in binary — supports L0 Home networking lab.
- **Toy linear-algebra neural net** (no framework) — supports AI track.
- **Entropy/compression experiment** — Huffman coding to feel information theory.

See full ladder in [[Learning Paths & Projects#Project Roadmap]].
