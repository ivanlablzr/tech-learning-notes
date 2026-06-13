---
type: domain
tags: [domain, moc, devops]
---


The evolution of software *delivery* — automating the path from source code to reliably running systems. **Not "Docker"**: it's a culture that fuses **Dev** teams (design/build/ship software) with **Ops** teams (monitor, manage environments, fix issues) so delivery becomes fast, safe, and repeatable.

## What DevOps actually is

A set of **practices + processes + tools** that replace slow, error-prone manual delivery:

- **The continuous pipeline:** ideate → code → build → test → deploy → operate → improve, automated end to end.
- **Continuous Integration (CI)** — every change is automatically built and tested.
- **Continuous Delivery/Deployment (CD)** — every passing change is automatically released (safely: blue/green, canary).
- **Continuous Monitoring** — production is observed and feeds back into development.
- **Reproducibility** — provision/install automatically, test in cheap production-like environments, and rebuild systems quickly for disaster recovery.

**DevSecOps** shifts security left into the pipeline — *manage access* (network ACLs, **IAM**, API keys), *gain insight* (posture, compliance, threat detection), and *protect data* (app/container security, key management with BYOK/KYOK, encryption at rest and in transit).

## The five views

**Dependencies:** builds on [[07 Programming]], [[04 Operating Systems|OS/containers]], [[09 Cloud]], and [[05 Networking]]; depended on by [[11 SRE]] (reliability builds on delivery) and [[13 Architecture|Platform Engineering]] (self-service platforms).
```
Programming + OS + Cloud → DevOps → (SRE, Platform Engineering)
```
The arc: **version control → CI → CD → containers → Kubernetes → IaC (Terraform, AWS CDK, CloudFormation) → GitOps → observability → SRE → platforms.**

**Learning path:** Git internals & PR flow → CI → CD → containers (images/registries/runtimes) → **Kubernetes** (pods, services, ingress, config/secrets) → **IaC** (Terraform modules, state, plan/apply) → **GitOps** (ArgoCD/Flux) → observability (→ [[11 SRE]]).

**Projects:** a CI/CD pipeline + containers (lint→test→build→push→deploy, L6) → a GitOps Kubernetes platform with observability (L7) → Terraform a cloud environment (L8). See [[Learning Paths & Projects#Project Roadmap]].

## Bridge topics it owns

- **Kubernetes (Containers ↔ Distributed ↔ Cloud)** — declaratively run many containers reliably across many machines: a control plane + scheduler + **etcd** (Raft consensus) + a flat networking model. Multi-parent (containers + networking + [[12 Distributed Systems|consensus]] + [[04 Operating Systems|namespaces]]).
- **Terraform / IaC (DevOps ↔ Cloud)** — reproducible, reviewable, versioned infrastructure; manages **state** and detects **drift**.
- Links out to **containers** ([[04 Operating Systems]]) and **observability** ([[11 SRE]]). Full map in [[Knowledge Graph#Bridge Topic Map]].

Related: [[07 Programming]] · [[04 Operating Systems]] · [[09 Cloud]] · [[11 SRE]] · [[12 Distributed Systems]] · [[Home]]
