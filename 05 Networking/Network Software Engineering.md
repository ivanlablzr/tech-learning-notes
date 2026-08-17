---
type: note
tags: [networking, automation, netdevops, software, career, projects]
---

> [[05 Networking]] → the emerging fusion of **network engineering + software engineering** ("NetDevOps" / Network Software Engineer). Networks are increasingly *programmed*, not hand-configured. Pairs with [[07 Programming]], [[10 DevOps]], [[09 Cloud]], [[AI-assisted Networking]]; certification context in [[IT Certifications and Learning Resources per Domain]].


The job is changing: instead of CLI-configuring boxes one by one, networks are run as **software** — provisioned by APIs, automated with code, monitored with telemetry, deployed through CI/CD. The **Network Software Engineer** sits between the network engineer (who knows BGP/VLANs) and the software engineer (who builds maintainable systems). This note is the single home for that path and a project-driven roadmap to grow into it.

> **The shift:** manual CLI → **automation** (Ansible) → **APIs** (provisioning services) → **SDN / programmability** (controllers). Networking becomes infrastructure-as-code.

## The roadmap — build one platform, not isolated tutorials
The trick: don't study technologies in isolation — build **one growing "Network Automation Platform,"** each phase adding a component. A recruiter then sees a progression from network engineer → automation → infrastructure → network software engineer.

### Phase 0 — Networking foundation *(you likely have this)*
Be fluent in TCP/IP, VLANs, STP, OSPF, **BGP**, ACLs, NAT, [[DNS]], DHCP. **Deliverable:** a lab in EVE-NG/GNS3 (Cisco/Juniper/Arista), topology documented on GitHub. (Depth & certs: [[IT Certifications and Learning Resources per Domain]].)

### Phase 1 — Software engineering foundations
- **Linux** (processes, systemd, networking stack, filesystems, permissions) → [[Linux]]
- **Git** (branches, PRs, merging, GitHub workflows)
- **Python** (classes, modules, venvs, error handling, testing) → [[07 Programming]]
- **Data formats** (JSON, YAML) · **Docker** (containers, Dockerfiles, Compose) · **PostgreSQL + SQL** → [[08 Databases]]
- **Project 1 — Device Inventory System:** CRUD + search + export over PostgreSQL (Python/Docker/Git). *Shows SWE fundamentals.*

### Phase 2 — APIs & network programming
- **Network libraries:** Netmiko, NAPALM, Paramiko. · **REST APIs** (HTTP, auth, CRUD). · **FastAPI** (routes, validation, docs, async).
- **Project 2 — Network Provisioning API:** `POST /devices /vlans /interfaces`, `GET /backups` — VLAN/interface provisioning + config backup. *First true network-software-engineering project.*

### Phase 3 — Network automation
- **Ansible, Terraform**; config generation → deployment → validation → reporting.
- **Project 3 — Automated Deployment Engine:** `request → generate config → push → verify → report`.

### Phase 4 — Observability & monitoring
- **SNMP / streaming telemetry**, time-series (metrics, retention, aggregation), **Prometheus + Grafana**.
- **Project 4 — Network Health Dashboard:** interface utilisation, errors, drops, CPU/memory → dashboards, alerts, reports.

### Phase 5 — Production engineering
- **CI/CD** (GitHub Actions, testing pipelines, linting, automated deploy) → [[10 DevOps]]; logging; audit trails; unit + integration tests.
- **Upgrade projects:** add **audit logs** (`User X created VLAN 100`) and a **compliance checker** (naming/VLAN/security standards). *Makes them enterprise-grade.*

### Phase 6 — Cloud networking
- **VPCs, subnets, route tables, security groups**; transit gateways, load balancers, **Kubernetes networking** (AWS/GCP/Azure) → [[09 Cloud]].
- **Project 5 — Cloud Network Provisioner:** API takes `{"environment":"dev"}` → creates VPC/subnets/security-groups + docs & topology diagrams.

### Phase 7 — SDN & network programmability *(last)*
- **SDN concepts, OpenFlow**, controller architecture (ONOS, OpenDaylight).
- **Project 6 — Mini SDN Controller:** topology discovery → shortest-path route calc → policy engine (`voice → path A`). *Closest to a true Network Software Engineer role.*

### Phase 8 — A second language *(after Python is solid)*
**Go** (cloud/networking — heavily adopted) or **Rust** (modern systems/networking); also C++ / Java. → [[07 Programming]].

## The final portfolio — one ecosystem
```text
Network Automation Platform
├── Device Inventory      ├── Compliance Checker
├── Provisioning API      ├── Monitoring Dashboard
├── Automation Engine     ├── Audit Logging + CI/CD
└── Cloud Provisioner     └── (Mini SDN Controller)
```
One coherent GitHub ecosystem >> six disconnected tutorial repos.

## Related
[[05 Networking]] · [[07 Programming]] · [[10 DevOps]] · [[09 Cloud]] · [[08 Databases]] · [[AI-assisted Networking]] · [[IT Certifications and Learning Resources per Domain]] · [[16 Home Lab Projects]]
