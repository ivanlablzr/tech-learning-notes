---
type: note
tags: [programming, software-engineering, sdlc]
---

Programming is writing code that works; **software engineering is programming multiplied by time and people** — the discipline of building systems that stay correct, maintainable, and shippable as they grow, as requirements change, and as others (including future-you) work on them. This is the node between [[07 Programming|Programming]] and [[12 Distributed Systems]] in the [[Master Index — Technology Vault]].

## 1. The software development lifecycle (SDLC)

Every methodology is a different answer to "when do we find out we were wrong?"

`Requirements → Design → Implementation → Testing → Deployment → Maintenance`

- **Waterfall** — each phase completes before the next; assumes requirements are knowable upfront. Survives only where change is genuinely expensive (aerospace, medical, contracts).
- **Agile** — short iterations, working software over documents, feedback over prediction. **Scrum** (sprints, backlog, standups, retros) adds structure; **Kanban** (continuous flow, WIP limits) suits ops/support work. The point is shortening the feedback loop, not the rituals.
- **Requirements** split into *functional* (what it does) and *non-functional* (how well: performance, availability, security, cost — the "-ilities" that actually drive architecture, see [[13 Architecture]]).
- **Maintenance is most of the lifecycle.** ~60–80% of total cost is after v1 ships — which is why readability, tests, and docs are economic decisions, not aesthetics.

## 2. Design principles

Rules for managing the real enemy: **complexity**. Good design = low **coupling** (modules don't need each other's internals) + high **cohesion** (each module has one job).

| Principle | Meaning | Smell it prevents |
|---|---|---|
| **S**ingle Responsibility | one reason to change per module | god classes |
| **O**pen/Closed | extend behavior without editing tested code | shotgun surgery |
| **L**iskov Substitution | subtypes must honor the parent's contract | surprise behavior behind an interface |
| **I**nterface Segregation | many small interfaces > one fat one | implementing methods you don't need |
| **D**ependency Inversion | depend on abstractions, not concretions | hard-wired dependencies, untestable code |

Plus the pragmatics: **DRY** (one source of truth — but wrong abstraction is worse than duplication), **KISS**, **YAGNI** (don't build for imagined futures), **composition over inheritance**, **fail fast** (validate at boundaries — same instinct as NASA rule 7 in [[Programming Foundations]]), and **principle of least astonishment**.

## 3. Design patterns

Named, reusable solutions to recurring design problems (Gang of Four). Value is the shared vocabulary — "make it a Strategy" — not memorizing all 23. The ones you'll actually meet:

| Pattern | Category | Solves | Seen in |
|---|---|---|---|
| **Factory** | creational | create objects without hard-coding classes | plugin systems, DB drivers |
| **Singleton** | creational | exactly one instance | config, connection pools (careful: global state) |
| **Builder** | creational | construct complex objects step-by-step | query builders, HTTP clients |
| **Adapter** | structural | make incompatible interfaces fit | wrapping third-party APIs |
| **Facade** | structural | simple front over a complex subsystem | SDK design |
| **Decorator** | structural | add behavior without subclassing | middleware, Python `@decorators` |
| **Strategy** | behavioral | swap algorithms at runtime | pricing rules, retry policies |
| **Observer** | behavioral | notify many dependents of change | events, pub/sub (→ [[12 Distributed Systems|queues]] at scale) |
| **Command** | behavioral | requests as objects (queue/undo/log) | task queues, undo stacks |

Anti-pattern warning: pattern-itis. Patterns are for problems you *have*.

## 4. Testing

The testing pyramid — many fast cheap tests at the bottom, few slow expensive ones on top:

```
        E2E (user flows, real browser/API — slow, brittle, few)
      Integration (components + real DB/API — medium)
    Unit (one function/class, isolated — fast, thousands)
```

- **Unit tests** isolate logic with **test doubles** (mocks/stubs/fakes) for I/O; follow *arrange → act → assert*; test behavior, not implementation (tests that break on refactor are liabilities).
- **Integration tests** catch what units can't: wiring, SQL, serialization, config.
- **E2E** (Playwright/Selenium) proves the user's path works; keep the suite small.
- **TDD** (red → green → refactor) shines for pure logic and bug-fix regression tests ("write the test that fails because of the bug, then fix").
- **Coverage** is a floor, not a goal — 100% coverage proves lines *ran*, not that assertions were meaningful. Untested error paths are where production dies.
- Others worth knowing: **property-based testing** (Hypothesis — generate inputs, assert invariants), **regression suites**, load tests (k6/Locust), and **security testing** (SAST/DAST → [[Internet & Application Security]]). For LLM-based systems the analogue is **evals** ([[AI Products & Startup Engineering]] §5).

## 5. Code quality & maintenance

- **Code review / PRs** — the highest-ROI quality practice: catches bugs, spreads knowledge, enforces standards. Review for correctness, clarity, and security — not style (automate style away).
- **Technical debt** — deliberate shortcuts trade speed now for interest later; the failure mode is *unintentional* debt from unclear design. Track it, pay it down alongside features (the "boy-scout rule": leave code cleaner than you found it).
- **Refactoring** — behavior-preserving restructuring, safe only with tests (Fowler's catalog: extract function, rename, inline). Refactor before adding a feature to messy code, not after.
- **Static analysis & linting** — ruff/pylint (Python), ESLint (JS), clippy (Rust), SonarQube; type checkers (mypy, TypeScript) catch whole bug classes pre-runtime. Same spirit as NASA rule 10 ([[Programming Foundations]]): zero warnings.
- **Documentation** — READMEs (what/why/how-to-run), API docs from code (OpenAPI/docstrings), and **ADRs** (Architecture Decision Records: context → decision → consequences — the "why" that code can't express). Comments explain *why*, never *what*.

## 6. Collaboration & delivery

- **Git workflows:** trunk-based (short-lived branches, merge daily — pairs with CI) vs GitFlow (release branches — heavier). Small PRs review better. Commits: imperative mood, atomic, explain why.
- **Semantic versioning:** `MAJOR.MINOR.PATCH` = breaking / feature / fix. Breaking changes to a published **API contract** (the bridge topic [[07 Programming]] owns) are the expensive kind — version APIs deliberately (`/v1/`, deprecation windows).
- **CI/CD:** every push → build + lint + test automatically (CI); deploy through staging with **feature flags**, canary/blue-green releases, and instant rollback (CD). Pipeline details live in [[10 DevOps]]; the engineering habit is *keep main shippable at all times*.
- **Estimation & scope:** estimates are distributions, not dates; cut scope, not quality. Postmortems are blameless and produce action items ([[11 SRE]] culture).

## 7. Secure & reliable engineering (shift-left)

Security as an SDLC property, not a final audit — cross-reference [[06 Cybersecurity]] / [[Internet & Application Security]]:

- **Threat model at design time** (STRIDE: what can be spoofed/tampered/leaked?), not after launch.
- **Dependencies are your attack surface:** pin versions, scan (Dependabot/Snyk/`pip-audit`), beware typosquats; a lockfile is a security control.
- **Secrets management:** env vars → secret stores, never in git history; rotate on leak.
- **Input validation at every trust boundary** and **least privilege** for every component — the same principles as [[LLMs & Prompting]] §8, because an LLM app is just software with a stochastic component.
- **Supply chain:** signed commits/artifacts, SBOMs, reproducible builds (rising compliance demand).

## 8. The software landscape — domains, what they build, with what

The map of what programmers actually work on. Columns: what the domain produces, the dominant stack, and the vault note that covers it.

| Domain | What they build | Typical stack | Vault link |
|---|---|---|---|
| **Web frontend** | UIs in the browser: SPAs, dashboards, e-commerce | TS/JS, React/Vue/Svelte, Next.js, Tailwind, Vite | [[Languages & Applied Programming]] §2 |
| **Web backend** | APIs, business logic, auth, integrations | Python/FastAPI, Node, Go, Java/Spring; PostgreSQL, Redis; REST/GraphQL/gRPC | [[Languages & Applied Programming]] §2, [[08 Databases]] |
| **Mobile** | iOS/Android apps | Swift/SwiftUI, Kotlin/Compose; cross-platform: React Native, Flutter | — |
| **Desktop** | Native/hybrid apps: IDEs, tools, clients | Electron/Tauri (web tech), Qt (C++), .NET/WPF | — |
| **Systems** | OSs, drivers, compilers, runtimes, browsers, DB engines | C, C++, **Rust**; assembly at the edges | [[04 Operating Systems]], [[Programming Foundations]] |
| **Embedded / IoT** | Firmware for devices: sensors, vehicles, medical, industrial | C/C++, Rust; FreeRTOS/Zephyr; MQTT | [[07 Programming]] §embedded, [[Digital Logic & Microcontrollers]] |
| **Game dev** | Games, engines, simulations | C++/C# (Unreal/Unity/Godot), shaders (HLSL/GLSL) | — |
| **Data engineering** | Pipelines, warehouses, lakes — data made queryable | Python, SQL, Spark, dbt, Airflow, Kafka; Snowflake/BigQuery | [[Languages & Applied Programming]] §3, [[08 Databases]] |
| **ML / AI engineering** | Models in production: training pipelines, serving, LLM apps | Python, PyTorch, Hugging Face; vector DBs; LLM APIs + orchestration | [[Machine Learning & Deep Learning]], [[AI Products & Startup Engineering]] |
| **DevOps / platform** | The paved road: CI/CD, IaC, K8s platforms, internal tooling | Go, Python, Bash; Terraform, Docker, Kubernetes | [[10 DevOps]], [[11 SRE]] |
| **Security engineering** | Scanners, detection rules, C2/red-team tooling, AppSec fixes | Python (tooling), Go/Rust/C (implants, agents), YARA/Sigma | [[06 Cybersecurity]], [[Internet & Application Security]] |
| **Network programming** | Protocol implementations, network automation, packet tools | Python (Scapy, Netmiko/NETCONF), C/Rust (fast paths), Go | [[05 Networking]], [[07 Programming]] |
| **Blockchain** | Smart contracts, protocols, wallets | Solidity, Rust (Solana), Go (nodes) | [[Programming Foundations]] §P2P |
| **Scientific / HPC** | Simulations, numerical computing, research code | Python (NumPy/SciPy), Fortran/C++, CUDA, Julia | [[00 Mathematics]], [[03 Computer Hardware|GPUs]] |

Reading the table strategically: every domain is **(a problem area) × (the systems knowledge to model it) × (a stack)**. Stacks are the cheapest column to learn and the fastest to churn; the durable investments are the left two columns — which is the same lesson as [[AI Products & Startup Engineering]] §9: domain expertise + engineering fundamentals transfer, frameworks don't. For your trajectory, the highest-leverage rows are **web backend + security engineering + network programming + ML/AI engineering** — they compose into exactly the collect → organize → reason → act pipeline.

Related: [[Programming Foundations]] · [[Languages & Applied Programming]] · [[07 Programming|domain overview]] · [[10 DevOps]] · [[12 Distributed Systems]] · [[13 Architecture]] · [[AI Products & Startup Engineering]] · [[06 Cybersecurity]]
