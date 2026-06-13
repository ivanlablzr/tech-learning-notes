---
type: note
tags: [ai, networking, aiops, foundations]
---

AI in networking is basically using [[Machine Learning & Deep Learning|machine learning]] to help **run, manage, and defend computer networks** automatically, with less manual work and faster reactions than a human can give.

One important point: AI does **not** replace the core of the network. The packets are still moved by IP, TCP, and routing protocols exactly as before (this part is called the *forwarding plane*). AI works one level above that, on the part where the network is monitored and configured (the *management and control plane*). It reads the data the network is already producing and turns that data into decisions.

The simple way the industry describes it: *the AI does the heavy lifting, the human stays in control of the rules.*

---

## 1. How AI is implemented in networking

AI isn't a single product you plug in. It's a chain of steps added on top of the network you already have. Each step feeds the next one.

The four steps:

1. **Collect** — the network constantly sends out data about its own state: how much traffic there is, delays (latency), lost packets, whether devices are healthy, plus logs. This raw data is called *telemetry*.
2. **Learn** — the AI studies that data over time and learns what "normal" looks like, so it can recognize when something is off.
3. **Decide** — when something looks abnormal, the AI flags it, works out the likely cause, predicts what might happen next, and suggests a fix.
4. **Act** — the AI applies the fix itself (changes a setting, reroutes traffic, applies a policy) and then checks that the result matches what was wanted.

Two ideas make this loop actually work:

- **Intent-Based Networking (IBN)** — instead of configuring each device by hand, you simply tell the network *what you want* ("phone calls get priority, guests stay separated from staff"). The system figures out the configuration, applies it, and keeps checking that reality still matches what you asked for, fixing it if it drifts. So you *describe the goal* instead of *typing the commands*.
- **AIOps** ("AI for IT Operations") — the platform that ties it all together: it gathers all the events and alerts, removes the noise (so you're not buried in thousands of useless alarms), and drives the fixes.

In real life you don't usually build this yourself — it comes built into vendor products: **Cisco** (Catalyst Center, Meraki, ThousandEyes, AI Assistant), **HPE Juniper Mist AI** (its assistant is called Marvis), **Arista** (CloudVision), **Nvidia** (Spectrum-X, for AI data-center networks), **Palo Alto Networks** (security AI), and monitoring tools like **Datadog** and **Dynatrace**.

---

## 2. What it's used for

AI is applied to the same jobs network engineers already do — it just does them faster and earlier:

- **Monitoring** → watches every metric all the time and spots small problems a human would scroll past.
- **Troubleshooting** → finds the *real* cause, not just the symptom (example: the Wi-Fi seems slow, but the actual problem is a DHCP server handing out addresses too slowly).
- **Prediction** → warns you *before* something breaks — a failing device, a link about to get congested, storage about to fill up.
- **Optimization** → improves performance by choosing better paths, balancing traffic, tuning Wi-Fi, and giving priority to important apps like calls and video.
- **Security** → detects attacks and strange behavior such as malware, DDoS floods, or an unknown device that shouldn't be there (see [[Network Security]], [[Security Operations & IR]]).
- **Automation** → carries out the routine work itself: configuring devices, applying policies, rerouting, and fixing common issues.
- **Assistants (GenAI copilots)** → you can ask a plain-English question like *"why is the Paris office slow?"* and get an answer, plus a suggested configuration.

---

## 3. Networking before vs. after AI

The clearest way to see the change is side by side:

| | Before AI | After AI |
|---|---|---|
| **Approach** | reactive — you fix things after users complain | proactive — you fix things before users notice |
| **Watching the network** | a person stares at dashboards | models watch everything and alert you |
| **Finding problems** | manual detective work based on experience | the system points to the cause automatically |
| **Configuring** | log into each device and type commands | state your goal; the system configures it |
| **The data** | slow, occasional checks (SNMP polling) | constant, second-by-second streaming telemetry |
| **How far it scales** | limited by how many devices one person can handle | software-driven, scales far past a human |
| **Time to fix** | hours of reading logs | minutes, sometimes seconds |

The deeper change is this: the network used to be **a pile of boxes you send commands to**, and it became **a system you tell your goal to, which then keeps itself in that state**. Network and security work also started using the same tools, and managing the network became more like writing software — saved, tested, and version-controlled like code (this approach is called *NetDevOps*).

---

## 4. How it changes the routine of an engineer not yet using AI

For someone who works the traditional, manual way, the job doesn't disappear — but the daily routine shifts from *doing the work by hand* to *supervising a system that does it*:

| The old routine                                  | The new routine                                             |
| ------------------------------------------------ | ----------------------------------------------------------- |
| log into devices and read command output by hand | review the problems and causes the AI outputs               |
| wait for a ticket, then chase the symptom        | act on the AI's warning before a ticket exists              |
| configure each device manually                   | set the policy once and let it apply automatically          |
| memorize commands and vendor syntax              | check data quality and write automation                     |
| be the only person who knows how it all works    | decide *when to trust* the system and when not to           |
| react to outages                                 | set the safety limits so automation can't make things worse |

What this means in practice: the manual skills don't become useless. You still have to understand IP, TCP, and routing deeply — because **you can't supervise something you don't understand**. But the *value* of the job moves from typing commands to **setting the goals, writing the automation, and judging the AI's decisions**. An engineer who only knows how to type commands is competing against a machine that types them faster. The safer path is to become the person who decides *what* the network should do and *where the limits are*.

One honest caveat: "AI runs the network by itself" is still more promise than reality. There's a scale from fully manual (L0) to fully self-driving (L5), and in 2026 most setups sit around the middle (L2–L3). The AI is only as good as the data it's fed, engineers rarely let it act completely on its own, and attackers can sometimes fool the detection models with carefully crafted traffic (see [[Internet & Application Security#AI and Machine Learning Security|AI/ML security]]). The human is still firmly in the loop — the job is just different.

---

## 5. How to actually implement it in a company

The honest short answer: for almost every company, you **buy it**, you don't build the AI yourself. Training the machine-learning models needs huge amounts of data and specialists, so providers do that part for you. Your job is to choose the right product, connect your network to it, and configure the rules — not to write the AI.

There are three realistic routes:

| Route | What it means | Who it's for |
|---|---|---|
| **Buy a vendor platform** (recommended) | the AI is already built into the product; you connect your devices and set policies | almost everyone — small business to large enterprise |
| **Build your own** | assemble open-source tools (telemetry collectors, a database, dashboards, your own models) on your own servers | large orgs with a data-science/automation team and a special reason to |
| **Hybrid** | buy the AI platform but keep your data and some processing on your own servers | regulated industries with strict data rules |

### The "buy" route in practice

Most AI networking today is sold as a **cloud-managed service** (the vendor runs the AI in their cloud) or as a **controller you run on a server in your own data center**. You don't write the AI; you do this:

1. **Pick a platform that matches your gear.** The AI usually works best with that vendor's own equipment, because it needs deep access to the devices. Common choices: **Cisco Meraki** (fully cloud-managed, simplest to start), **Cisco Catalyst Center** (a controller you run for larger campus networks), **HPE Juniper Mist** (cloud-managed, strong on Wi-Fi and its Marvis assistant), **Arista CloudVision** (data center). For the security side, tools like **Palo Alto**, **Darktrace**, or a **SIEM/XDR** add AI threat detection (see [[Security Operations & IR]]).
2. **Connect your devices.** Point your routers, switches, firewalls, and access points at the platform so they stream their telemetry to it. With cloud-managed gear this is mostly automatic; with a self-hosted controller you install it on a server (or virtual machine) first.
3. **Let it learn.** Give it a few weeks to build a baseline of what "normal" looks like on your network before you trust its alerts.
4. **Set your intent and policies.** Tell it your goals — which apps get priority, who can talk to whom, what counts as suspicious — and it enforces them.
5. **Turn on automation gradually.** Start in *"recommend only"* mode (it suggests, you approve), and only let it act on its own once you trust it for specific, low-risk fixes.

### Do you need your own server?

It depends on the route:

- **Cloud-managed product** (e.g. Meraki, Mist) → **no server needed for the AI.** The vendor runs it; you just manage it through a web dashboard. Fastest and easiest, but your telemetry lives in their cloud and there's a subscription cost.
- **Self-hosted controller** (e.g. Catalyst Center) → **yes, you run it on your own server or VM**, but you're still installing *their* software, not building the AI. More control, more maintenance.
- **Fully build-your-own** → yes, your own servers and a team — only worth it for special cases.

### What this means for your three goals

- **Monitoring** → easiest win and the right place to start. Turn on the platform's AI monitoring and let it baseline and alert. Low risk because it only watches.
- **Management/automation** → roll out next, carefully. Begin with intent-based policies and *recommend-only* automation, then allow automatic fixes for safe, well-understood tasks.
- **Security** → can run on the same platform or a dedicated security tool. Treat its automatic actions (like blocking traffic) with extra caution, since a wrong block can cut off real users.

A sensible first project: pick **one site or one network layer** (often the Wi-Fi, since the AI assistants are strongest there), put it on a cloud-managed platform in monitoring mode, learn to trust it, then expand. You don't need to rebuild everything at once.

---

## One-line summary

AI in networking is a **collect → learn → decide → act** loop added on top of the network, used to monitor, troubleshoot, predict, optimize, secure, and automate. It turns networking from reactive, type-the-commands work into proactive, describe-your-goal work — and moves the engineer's role from doing the tasks by hand to setting the rules and supervising the system.

Related: [[Network Performance & Resilience]] · [[Network Security]] · [[Security Operations & IR]] · [[Internet & Application Security#AI and Machine Learning Security|AI/ML security]] · [[Machine Learning & Deep Learning]] · [[14 AI|domain overview]]
