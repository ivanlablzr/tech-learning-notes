---
type: domain
tags: [domain, moc, cloud]
aliases: [IAM]
---


Renting and composing datacenter capacity as **programmable, on-demand services** — from hypervisors to global platforms. It trades slow, capital-heavy hardware ownership for elastic pay-as-you-go infrastructure.

## Domain notes

| Note | Covers |
|---|---|
| [[Cloud & Datacenters]] | NIST model, IaaS/PaaS/SaaS, storage types, CDN, cloud-managed LAN, datacenter fabric |

## The five views

**Dependencies:** builds on [[03 Computer Hardware|virtualization]], [[05 Networking]], [[06 Cybersecurity|security/IAM]], and [[12 Distributed Systems|distributed systems]] (HA, replication); depended on by [[10 DevOps]], [[13 Architecture]], and [[14 AI]] (training/inference infra).
```
Hardware + Networking + Security + Distributed → Cloud → (DevOps, Architecture, AI)
```
The arc: **virtualization → VMs → cloud services → IaC → containers → orchestration → multi-account → multi-region → global platform.**

**Sub-areas:** compute (VMs/containers/serverless), storage (object/block/file), networking (VPC, CIDR, peering/transit, private endpoints), identity (IAM/federation/SSO), governance (landing zones, organizations, SCP guardrails), and **FinOps** (cost/unit economics).

**Learning path:** virtualization → compute → storage → cloud networking (VPC/CIDR) → **IAM** → automation (IaC/[[10 DevOps|Terraform]]) → multi-account governance → multi-region HA/DR → FinOps & a Well-Architected review lens.

**Projects:** single-VPC web app (compute + storage + IAM + load balancer) → multi-account landing zone (L8) → multi-region resilient system (L9). See [[Learning Paths & Projects#Project Roadmap]].

## Bridge topics it owns

**IAM (Identity & Access Management) — Security ↔ Cloud ↔ Architecture.** The control plane for *who may do what to which resource* at scale. Core pieces: **identities** (users, groups, **roles**, service/workload identities), **policies** (allow/deny rules — prefer least privilege), **authentication** (often MFA + **federation/SSO** so corporate identities map to cloud roles), and **signed, short-lived tokens** for authorization. It's multi-parent — built on [[06 Cybersecurity|identity/auth]] + [[Cryptography|crypto]] (token signing) + federation. Misconfigured IAM (over-broad roles, leaked keys, public buckets) is the #1 cloud breach cause.

**Load balancing — Networking ↔ Cloud ↔ Distributed.** Spread traffic for scale and availability: **L4** (transport) vs **L7** (application-aware), with health checks and global routing.

Also links out to **hypervisors/virtualization** ([[03 Computer Hardware]]) and **Terraform/IaC** ([[10 DevOps]]). Full map in [[Knowledge Graph#Bridge Topic Map]].

Related: [[03 Computer Hardware]] · [[05 Networking]] · [[06 Cybersecurity]] · [[10 DevOps]] · [[12 Distributed Systems]] · [[Home]]
