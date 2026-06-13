---
type: note
tags: [cloud, datacenters, cdn]
---


The on-demand delivery of compute, storage, and services over the internet — and the physical datacenters and edge networks that make it fast and global.

## 1. Cloud fundamentals

Cloud shifts IT from **CapEx** (buy hardware) to **OpEx** (pay for what you use). NIST's five essential characteristics: **on-demand self-service**, **broad network access**, **resource pooling** (multi-tenant), **rapid elasticity**, and **measured service** (metered/billed).

**Service models (SPI)** — each shifts more management to the provider:

| Model    | You manage              | Examples                              |
| -------- | ----------------------- | ------------------------------------- |
| **IaaS** | OS, runtime, data, apps | EC2, Azure VMs, GCE                   |
| **PaaS** | app code + config       | Elastic Beanstalk, Heroku, App Engine |
| **SaaS** | just your settings/data | Microsoft 365, Salesforce, Dropbox    |

Compute options span **VMs**, **bare metal**, and **serverless** (Lambda/Functions — event-driven microservices, no server management).

**Deployment models:** **public** (shared, third-party — a private isolated subnet inside it = **VPC**), **private** (single-org, max control/compliance), **hybrid** (private + public, e.g. sensitive data on-prem, burst traffic to public), **multi-cloud** (mix providers by strength/cost).

**Scaling & resilience:**
- **Scalability** — handle more work by adding resources: **vertical** (scale *up*: more CPU/RAM on one server) vs **horizontal** (scale *out*: more servers).
- **Elasticity** — scale out/in *automatically* with real-time demand.
- **HA** (instant failover, zero downtime) and **fault tolerance** (survive component failure), achieved across **Regions** (geographic) and **Availability Zones** (physically separate datacenters within a region). All of it rests on **virtualization** ([[03 Computer Hardware|hypervisors]]).

> Providers: AWS, Azure, GCP (the big three), plus Oracle, IBM, OVHcloud, Alibaba — each stronger/cheaper for different workloads.

## 2. Cloud storage types

| Type | Structure | Access | Best for | Trade-off |
|---|---|---|---|---|
| **DAS** (direct-attached) | local disk | local bus (NVMe/SATA) | fastest, lowest latency | can't be shared |
| **File / NAS** | folder hierarchy | NFS/SMB over Ethernet | shared app files (multi-node) | network overhead |
| **Block / SAN** | fixed-size blocks, no metadata | iSCSI/Fibre Channel | OS disks, databases (high IOPS) | usually single-node, costly |
| **Object** | flat buckets + rich metadata | HTTP REST (`GET`/`PUT`) | unstructured data at scale, cheap, tiered (hot→cold/archive) | rewrite whole object to change it |

Performance is measured in **IOPS** (I/O operations per second).

## 3. CDN — content delivery networks

A globally distributed network of caching **edge servers (PoPs)** that serve content from near the user, cutting latency and offloading the **origin server**.

- **Caching:** a **cache hit** serves locally; a **cache miss** fetches from origin, caches it, then serves; **TTL** sets how long before content is stale. **Static** content (images/CSS/video) caches well; **dynamic** per-user content uses route optimization (DSA) instead.
- **Routing strategies:** **Anycast** (same IP from many sites — Cloudflare), **DNS-based** (Akamai), **embedded ISP cache** (Netflix Open Connect).
- **Security shield** (the edge is the outer perimeter): **DDoS absorption**, **WAF** (block SQLi/XSS at the edge), **TLS offload**, and **origin cloaking** (hide the real origin IP). Providers: Cloudflare, Akamai, Fastly, CloudFront.

## 4. Cloud-managed LAN

Network gear (switches, APs) configured and monitored through a **vendor-hosted cloud dashboard** rather than per-device CLI. The on-prem hardware keeps forwarding (data plane) locally; the cloud handles config, telemetry, and firmware (control plane).

- **Zero-touch provisioning (ZTP):** plug a device into the internet and it auto-registers and pulls its config — non-technical staff can deploy hundreds of sites.
- **Benefits:** centralized templates, fleet-wide visibility/AIOps, automatic updates, fast rollout. **Trade-offs:** internet/management dependency, **subscription licensing**, **vendor lock-in**, and **data-sovereignty** concerns (telemetry in vendor cloud). If the cloud is unreachable, local traffic continues but config changes and (cloud-RADIUS) new-client auth may not.
- Vendors: Cisco Meraki, Juniper Mist, Aruba Central, Ubiquiti UniFi (self-hostable, no per-device fee).

## 5. Datacenter architecture

Datacenters house **servers, storage, and switching** plus power and cooling. Modern designs:

- **Spine-leaf fabric** replaces the old three-tier design: every leaf connects to every spine → predictable latency and massive horizontal scale for **east-west** (server-to-server) traffic.
- **SDN** splits the **control plane** (routing logic, centrally programmable) from the **data plane** (forwarding hardware); **SD-WAN** applies the same idea across WAN links (dynamically route over fiber/broadband/LTE by cost and performance).
- **Underlay vs overlay:** the underlay is the physical switches + routing (OSPF/BGP); the **overlay** is a virtual network tunneled on top (**VXLAN**, WireGuard) — how multi-tenant cloud networking and container fabrics isolate traffic.
- Operators: Equinix, Digital Realty, NTT, CyrusOne, Vantage (colocation/wholesale).

Related: [[09 Cloud|domain overview]] · [[03 Computer Hardware]] · [[Network Infrastructure]] · [[Switching & Routing]] · [[10 DevOps]] · [[IAM]]
