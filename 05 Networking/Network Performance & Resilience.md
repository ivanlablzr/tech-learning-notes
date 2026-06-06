---
type: note
tags: [networking, performance, resilience, raid]
---


Measuring how fast a network is, and designing it so failures don't take it down.

## 1. Performance metrics

- **Bandwidth** — the *potential* transmission rate (bps/Mbps/Gbps), limited by the slowest of NIC, link, and device ports.
- **Throughput** — the *actual* rate successfully delivered, reduced by congestion, overhead, and hardware limits.
- **Latency** — time for a packet to travel source→destination (ms). It's the sum of four delays: **processing** (router examines the header), **queuing** (waiting in a buffer), **transmission** (pushing bits onto the link), and **propagation** (signal traveling the medium).
- **Jitter** — variation in latency over time (ms); kills real-time apps (VoIP/video).
- **Packet loss** — from congestion, protocol overhead, or hardware faults.

> **Measure with:** `ping` (latency + loss via ICMP), `traceroute` (per-hop path/latency), `iperf` (bandwidth), and NMS/flow tools (Wireshark, NetFlow, SolarWinds/PRTG/Nagios). Congestion sources to watch: queuing delay, broadcast storms (a loop with no STP), MAC flooding, ARP spoofing.

## 2. Resilience: no single point of failure

Resilience rests on **redundancy** (duplicate components), **fault tolerance** (keep working when one fails), **failover** (auto-switch to backup), and **high availability** (uptime, e.g. "five nines" = 99.999%).

**Layer 2:**
- **STP/RSTP** prevent loops/broadcast storms by blocking redundant links into a loop-free tree; RSTP converges in ~1–2 s (vs STP's 30–50 s). Downside: blocked links waste bandwidth. Speed it up with PortFast (access ports) + BPDU Guard.
- **Port-Channel / EtherChannel (LACP)** bundles links into one logical link — redundancy *and* aggregated bandwidth, with seamless failover (no STP reconvergence).

**Layer 3:**
- **ECMP** load-balances across equal-cost routes (per-flow hash) and drops failed paths automatically.
- **Floating static routes** — a backup route with higher administrative distance that activates only when the primary disappears (good for dual-ISP).
- **FHRPs** give end devices a redundant default gateway via a shared **virtual IP/MAC**:

| FHRP | Standard | Load balancing |
|---|---|---|
| **HSRP** | Cisco | no (active/standby) |
| **VRRP** | open (RFC 5798) | no (master/backup) |
| **GLBP** | Cisco | yes |

**WAN:** **dual ISP** (active/active needs BGP; active/passive is simpler), **BGP failover** (advertise your prefix to both providers), **BFD** (sub-second failure detection — ~150–900 ms vs slow routing timers), and **SD-WAN** (multiple active transports, app-aware reroute in ms — see [[Wireless & Cellular]]).

**Application layer:** **load balancers** spread connections across a server pool with **health checks** (L4 = TCP/UDP hash/round-robin; L7 = route by URL/header/cookie; e.g. HAProxy, NGINX, F5, AWS ALB), and **anycast** (same IP from many datacenters — clients hit the nearest; BGP withdraws a failed site). 

## 3. Disaster recovery

Two targets define DR: **RTO** (Recovery Time Objective — max acceptable *downtime*) and **RPO** (Recovery Point Objective — max acceptable *data loss*, which sets backup frequency).

| Tier | Approach | RTO | RPO |
|---|---|---|---|
| Cold standby | manual restore from backup | hours–days | hours–days |
| Warm standby | pre-built, needs activation | minutes–hours | minutes |
| Hot standby | real-time sync, auto failover | seconds–minutes | seconds |
| Active-active | fully redundant, load-shared | ~zero | ~zero |

Backups: full / differential / incremental snapshots (a snapshot = point-in-time image); automate, monitor, log, and **validate** them, and keep copies offsite (3-2-1 rule).

## 4. RAID — disk-level redundancy

| Level | Method | Survives | Note |
|---|---|---|---|
| RAID 0 | striping | nothing | speed only |
| RAID 1 | mirroring | 1 disk | 50% usable |
| RAID 5 | striping + parity | 1 disk | ≥3 disks |
| RAID 6 | striping + dual parity | 2 disks | ≥4 disks |
| RAID 10 | mirror + stripe | 1 per pair | best speed + resilience |

> **RAID is not a backup** — it survives disk failure, not deletion, ransomware, or site disaster.

**Design checklist:** redundant core (switches/links/power), L2 loop prevention, FHRP gateways, dual access uplinks, dual-ISP/SD-WAN, BFD on critical links, load balancing + health checks, and tested RTO/RPO for every critical system.

Related: [[Switching & Routing]] · [[Wireless & Cellular]] · [[Network Devices]] · [[Network Security]] · [[05 Networking|domain overview]]
