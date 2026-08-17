---
type: reference
tags: [networking, observability, monitoring, tools, sre, security]
domains: [networking, distributed-systems, cybersecurity]
maturity: growing
updated: 2026-08-05
---

> [!abstract] What this note is
> The **running catalog** of network monitoring & observability tools — each with *what it is*, *when to use it*, and *which layer of the pipeline* it sits in. A reference to pull from, not a concept explainer (mechanisms → future [[05 Networking|Networking]] concept notes). Add rows as you meet new tools.

> [!tip] How to read it
> The **pipeline** is: **collect → transport → store → visualise → alert**. Every tool below fits one (or spans several) of those stages. The **Domain** column shows who consumes it — `Net` (NOC/network eng), `SRE` (ops/observability), `Sec` (SOC/NSM). Tools tagged multiple = the shared firehose a NOC and SOC both drink from.

---

## 1. Collection — getting data *off* the devices

| Tool / Protocol | Type | Usage context — *when you reach for it* | Domain |
|---|---|---|---|
| **SNMP** | Poll (pull) | The legacy default — counters, interface stats, traps. Everywhere, but coarse & slow-polling. Use when the device is old or that's all it speaks | Net |
| **gNMI** | Streaming telemetry (gRPC) | Modern model-driven telemetry; subscribe to state and get pushed updates. Use on current gear (Arista/Cisco/Juniper) instead of SNMP polling | Net |
| **gNMIc** | gNMI CLI/collector | The go-to client to *subscribe, capture and export* gNMI streams (to Prometheus, Kafka, InfluxDB). Your hands-on tool for streaming telemetry | Net |
| **YANG / OpenConfig** | Data models | Not a tool — the schema gNMI/NETCONF speak. Learn it to know *what* you can subscribe to | Net |
| **Telegraf** | Agent/collector | Plugin-based collector (SNMP, gNMI, system metrics) → pushes to InfluxDB/Prometheus. Use as a universal input adapter | Net · SRE |
| **node_exporter / SNMP exporter** | Prometheus exporters | Translate host/SNMP metrics into Prometheus format. Use to pull device/host metrics into a Prom stack | SRE · Net |

## 2. Flow telemetry — *who talked to whom, how much*

| Tool / Protocol | Type | Usage context | Domain |
|---|---|---|---|
| **NetFlow / IPFIX** | Flow records | Full(ish) flow accounting — src/dst/ports/bytes. Use for traffic analysis, billing, and **security** (exfil/C2 detection) | Net · Sec |
| **sFlow** | Sampled flow | Packet *sampling* at line rate — lighter than NetFlow, great on high-throughput switches. Use when you need scale over completeness | Net · Sec |
| **ntopng** | Flow analyser | Visualise NetFlow/sFlow, per-host traffic, DPI. Use for quick "what's eating the link / who's weird" | Net · Sec |
| **pmacct** | Flow collector | Collect/aggregate NetFlow/sFlow/IPFIX → DB. Use to build your own flow pipeline | Net |

## 3. Control-plane monitoring

| Tool / Protocol | Type | Usage context | Domain |
|---|---|---|---|
| **BMP** (BGP Monitoring Protocol) | Streaming | Watch BGP RIBs/updates **without touching the router** or peering with it. Use for route monitoring, hijack/leak detection, troubleshooting | Net · Sec |
| **OpenBMP / obmp** | BMP collector | Ingest BMP feeds → DB/Kafka for analysis. Pair with BMP above | Net |

## 4. Storage — time-series & logs

| Tool | Type | Usage context | Domain |
|---|---|---|---|
| **Prometheus** | Metrics TSDB (pull) | The default metrics store + scraper + query lang (PromQL). Use as the heart of a metrics stack; pull model, great for dynamic infra | SRE · Net |
| **InfluxDB** | Metrics TSDB (push) | Push-model TSDB, pairs with Telegraf. Use when push fits better (network telemetry, IoT) | SRE · Net |
| **VictoriaMetrics / Thanos / Mimir** | Long-term Prom storage | Scale Prometheus (long retention, HA, multi-cluster). Use when a single Prom isn't enough | SRE |
| **Loki** | Log store | Grafana's log backend — "Prometheus for logs", label-based. Use for cheap log aggregation alongside Grafana | SRE · Sec |
| **Elastic / OpenSearch (ELK)** | Log/search store | Heavy full-text log analytics; the classic **SIEM** substrate. Use for deep log search & security analytics | Sec · SRE |
| **Graylog** | Log management | Simpler ELK alternative for centralised syslog. Use for straightforward log centralisation | Sec · Net |

## 5. Visualisation

| Tool | Usage context | Domain |
|---|---|---|
| **Grafana** | The universal dashboard — reads Prometheus, InfluxDB, Loki, Elastic, etc. Your single pane of glass. Default choice | SRE · Net · Sec |
| **Kibana** | Elastic's dashboard — use when your data lives in ELK | Sec |

## 6. Alerting

| Tool | Usage context | Domain |
|---|---|---|
| **Alertmanager** | Prometheus's alert router — dedup, grouping, silences, routes to email/Slack/PagerDuty. Use with any Prom stack | SRE · Net |
| **Grafana Alerting** | Alerts defined in Grafana itself — use if you want alerting without a separate Alertmanager | SRE |

## 7. All-in-one NMS (batteries included)

| Tool | Usage context | Domain |
|---|---|---|
| **Zabbix** | Full NMS: polling, triggers, dashboards, alerting in one. Use for classic infra/network monitoring without assembling a stack. **Employer-demanded** (your [[IT & Cyber Job Market — Skills Employers Want|market notes]]) | Net · SRE |
| **LibreNMS** | Auto-discovering SNMP NMS — great for network gear. Use for quick network-device visibility | Net |
| **Nagios / Icinga** | Host/service up-down checks, the old guard. Use for availability monitoring | Net · SRE |
| **Observium / Cacti / PRTG** | SNMP graphing/NMS variants | Net |
| **Netdata** | Per-node real-time, zero-config. Use for instant single-host deep metrics | SRE |

## 8. Distributed tracing (app/service layer)

| Tool | Usage context | Domain |
|---|---|---|
| **OpenTelemetry** | The vendor-neutral standard for metrics+logs+traces instrumentation. Learn this — it's where the field is converging | SRE |
| **Jaeger / Tempo** | Trace storage/UI — use to follow a request across microservices | SRE |

## 9. Security-oriented monitoring (NSM — the SOC's view)

*Same packets, different question: not "is it healthy" but "is it hostile."* These **consume** the telemetry above — see [[Security Operations & IR]] & [[Digital Forensics & Anti-Forensics]].

| Tool | Usage context | Domain |
|---|---|---|
| **Zeek** (Bro) | Turns traffic into rich connection/protocol logs. The backbone of network security monitoring | Sec · Net |
| **Suricata** | IDS/IPS + flow/PCAP — signature & anomaly detection on the wire | Sec · Net |
| **Wireshark / tshark** | Deep packet inspection for troubleshooting & forensics. The scalpel | Net · Sec |
| **SIEM** (Splunk / Elastic / Wazuh) | Correlate all logs/flows into detections & alerts. The SOC's cockpit | Sec |

---

## How the pieces fit (the one-line mental model)

> **Collect** (SNMP/gNMIc/sFlow/BMP) → **transport** (Telegraf/Kafka) → **store** (Prometheus/InfluxDB/Loki/Elastic) → **visualise** (Grafana) → **alert** (Alertmanager). Zabbix/LibreNMS bundle all five; Zeek/Suricata/SIEM re-ask every stage as *"is this an attack?"*

Build a slice of this in your [[16 Home Lab Projects|home lab]]: **gNMIc → Prometheus → Grafana → Alertmanager** on a virtual router is a portfolio-grade project that hits your [[IT & Cyber Job Market — Skills Employers Want|employer-demanded]] supervision skills.

## Connections
- [[05 Networking]] — the owner domain; telemetry is how you *see* the network
- [[Network Performance & Resilience]] — what you monitor *for* (latency, loss, capacity)
- [[11 SRE]] · [[10 DevOps]] — own the generic Prometheus/Grafana/Alertmanager pipeline
- [[Security Operations & IR]] · [[Digital Forensics & Anti-Forensics]] — consume this telemetry as NSM/detection
- [[Cloud & Datacenters]] — cloud-native monitoring (CloudWatch, managed Prometheus) is the same model
- [[16 Home Lab Projects]] — where you build a working slice
- [[IT & Cyber Job Market — Skills Employers Want]] — supervision (Zabbix/Prometheus/Grafana) is repeatedly demanded

## Related
[[05 Networking]] · [[Master Index — Technology Vault]]
