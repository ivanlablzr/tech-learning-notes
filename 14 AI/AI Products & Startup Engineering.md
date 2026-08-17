---
type: note
tags: [ai, products, startups, engineering]
---

How AI becomes a product — the architecture of modern AI startups, why software engineering matters more than model training, what actually creates defensibility, and the realistic path from domain expertise to a working AI business.

## 1. The core misconception

The naive mental model is `train an AI → sell it`. Almost no company does this. Training a frontier model costs $10M–500M+ (see [[14 AI]]) and is pursued by fewer than ~20 organizations worldwide. The real pattern:

```
Customer problem → Software → Existing AI (API) → Product
```

**The product is the software; the LLM is one component.** GPT/Claude are platforms the way Linux and Windows are platforms — Cursor is not Gemini, Harvey is not Claude, a cloud-security startup is not GPT. The startup's job is making a general reasoning engine useful for one specific, valuable workflow.

| Layer | Who builds it | Examples |
|---|---|---|
| **Foundation models** | ~20 labs | OpenAI, Anthropic, Google, Meta, Mistral, DeepSeek |
| **Infrastructure** | specialized vendors | vector DBs, eval platforms, model routing, GPU clouds |
| **Applications** | *almost all startups live here* | Cursor, Harvey, Abridge, security copilots |

## 2. Anatomy of an AI product

```
                 Customer
                    │
            Web / Mobile App
                    │
               Backend API
         ┌──────────┴──────────┐
   Business logic        AI Orchestrator ──► LLM API (Claude/GPT/local)
         │                     │
   Databases / cache      Tool calling
         └──────────┬──────────┘
                    │
        Collectors & integrations
     (AWS, M365, GitHub, SIEM, Slack…)
```

Only one box is "AI." Everything else is [[07 Programming|software engineering]]: APIs, auth, [[08 Databases|databases]], queues, caching, monitoring, billing. Without it there is no product — only an API call.

## 3. The four-stage pipeline: Collect → Organize → Reason → Act

Nearly every AI product performs these four jobs. **This pipeline is the product; the LLM only does stage 3.**

### 3.1 Collect
Software (not the LLM) gathers raw data through APIs with proper authentication — CloudTrail logs, firewall configs, M365 audit logs, tickets, documents. This is pure [[06 Networking|networking]] + API engineering: OAuth flows, pagination, rate limits, retries, incremental sync. Beginners imagine ChatGPT "logging into AWS"; in reality a **collector service** (Python/Go) pulls data into an internal database on a schedule. The collector is unglamorous and is often 40%+ of the engineering effort.

### 3.2 Organize
Raw data is nearly useless to an LLM — 4 TB of logs won't fit in any context window, and most of it is noise. The organize stage turns raw data into **structured, queryable knowledge**:

```
Raw logs → normalized tables → relationships → (knowledge graph / embeddings) → retrievable context
```

Concretely: a CloudTrail event becomes `Alice → assumed role → Admin → accessed S3 → downloaded customer DB` — an *attack path*, not a log line. This is where much of the real value lives, and why AI startups are full of **PostgreSQL** (source of truth), **pgvector/Chroma/Pinecone** (embeddings for [[LLMs & Prompting|RAG]]), **Neo4j** (relationship/attack-path queries), and **Elasticsearch** (search over logs). Choosing *what to model and which relationships matter* is a domain-expertise decision, not an AI decision.

### 3.3 Reason
Only now is the LLM used — on **carefully selected context**, never on raw firehoses. The software asks narrow, high-value questions: "rank these 12 findings by exploitability," "explain why this attack path is dangerous to a non-technical owner," "does this config violate CIS benchmark X?" Small context, specific task, verifiable output.

### 3.4 Act
The LLM's reasoning turns into work: draft the Jira ticket, generate the Terraform fix, write the incident summary, produce the compliance report — or, via **tool calling**, trigger software that acts (run a scanner, query a firewall). The LLM never touches infrastructure directly; it *requests* actions and your software executes them under [[Internet & Application Security|least privilege]] with approval gates.

## 4. The orchestrator — where engineering meets AI

The orchestrator is the most important component you'll write. It decides:

- **what data to retrieve** (RAG query, DB lookup, graph traversal)
- **what tools to expose** and when to allow them
- **what prompt to build** (system role, context assembly, output schema)
- **which model to use** (cheap model for extraction, flagship for hard reasoning)
- **how to validate the answer** before anything downstream trusts it

A minimal agent loop (see [[LLMs & Prompting]] §7): send prompt + tool definitions → model emits a tool call → *your code* executes it → feed result back → repeat until the model produces a final answer. Frameworks (LangChain, LlamaIndex) exist, but a direct SDK loop is often simpler, more debuggable, and forces you to actually understand the flow.

**Prompts are code.** Version them, test them, review changes like any other diff. A prompt edit can silently break production the same way a code change can.

## 5. Reliability engineering — the invisible 80%

This is what separates a weekend demo from a product, and it's what most explanations (including polished AI-generated ones) undersell. LLMs are stochastic, they [[LLMs & Prompting|hallucinate]], and customers act on your output.

- **Structured output + validation:** request strict JSON, validate with Pydantic, retry on failure, `temperature=0` for extraction. Never regex-parse prose and hope.
- **Evals:** a test suite for AI behavior. Build a golden dataset (50–200 real cases with known-correct answers), score every prompt/model change against it. Without evals you cannot tell whether a change made things better or worse — you're guessing.
- **Guardrails / never-list:** define what the LLM is *never* allowed to decide. Access control, authorization, "is this safe to auto-remediate" → deterministic code, never model output (it can be coerced into emitting `{"authorized": true}` — see [[LLMs & Prompting]] §9).
- **Human-in-the-loop:** any destructive or costly action gets an approval gate. Start with everything gated; earn autonomy per-action as eval data accumulates.
- **Grounding + citations:** make the model cite which retrieved document/log supports each claim; unsupported claims get dropped or flagged.
- **Failure handling:** exponential backoff on rate limits, model fallback (provider outage ≠ your outage), logging of every prompt/response pair for debugging and audit.

Rule of thumb: getting from a 90%-reliable demo to a 99%-reliable product is most of the work, and it's evals + guardrails + data quality, not smarter prompts.

## 6. Economics — tokens are COGS

Unlike classic SaaS (near-zero marginal cost), every LLM call costs money. Unit economics decide viability:

- **Model tiering:** cheap models (Haiku/gpt-4o-mini) for classification, extraction, routing — flagship models only for the hard reasoning steps. Often a 10–30× cost difference.
- **Use the LLM sparingly on high-value decisions.** "Rank these 10 findings" is cheap and valuable; "analyze all my logs" is expensive and vague. The Organize stage exists partly to shrink what the LLM ever sees.
- **Prompt caching** (repeated system prompt/context prefixes), **response caching**, and **batch APIs** cut costs 50–90% for the right workloads.
- **Pricing:** price the *outcome* (assessment, report, monitored environment per month), never per-token — you want margin between customer value and API cost, and that margin is your business.

Startups that thinly resell reasoning get squeezed between API prices and customer willingness to pay. Startups that wrap reasoning in proprietary data + workflow keep margin.

## 7. Security of AI products

You're building in security, so your product must survive its own threat model (details in [[LLMs & Prompting]] §9 and [[Internet & Application Security|AI & ML Security]]):

- **Prompt injection** — especially *indirect*: your collector ingests a log/README/email containing hidden instructions, which your orchestrator then feeds to the model. Treat all collected data as untrusted input; separate system/user roles; filter; require approval for sensitive actions.
- **Least privilege everywhere:** the model is "a smart but unscrupulous intern" ([[14 AI]]) — read-only credentials for collectors, scoped tokens, sandboxed tool execution, full audit logs.
- **Data handling:** customer logs are crown jewels. Encryption, tenant isolation, retention policies, and a clear answer to "does our data train your model?" (contractually: no). For sensitive clients, local/open-weight models (Ollama + Llama/Qwen) can be a selling point.
- **Compliance as a feature:** SOC 2, insurer questionnaires, audit trails — painful, but for a security product, *being trustworthy is the product*.

## 8. Moats — what's actually defensible

"Anyone can call GPT" — so the model is never the moat. But be skeptical of the standard list too: foundation labs keep absorbing the layer above them (agents, coding assistants, deep research were all "startup categories"). What has held up:

| Moat | Why it survives model improvements |
|---|---|
| **Proprietary data** | collected from *your* customers' environments; no API has it |
| **Integrations depth** | hundreds of connectors + edge cases = years of grind |
| **Workflow lock-in** | embedded in the customer's daily process; switching is painful |
| **Trust & compliance** | SOC 2, insurer acceptance, track record — accrues to *you*, not the model |
| **Domain expertise** | knowing which data matters, how to model it, what "good" output looks like |
| **Distribution** | existing customer relationships and reputation |

Weak moats: prompts (copyable), thin UI wrappers (absorbed), "we use GPT-X" (everyone does). If a better model would make your product *stronger* rather than obsolete, you're on the right side.

## 9. Domain expertise is the differentiator

Two people build an "AI network engineer." Person A knows AI + Python + prompting. Person B knows BGP, OSPF, VLANs, firewalls, AD, AWS networking — *plus* enough software engineering. **Person B wins**, because every hard decision in the pipeline is a domain decision:

- *Collect:* which logs/configs actually matter?
- *Organize:* what schema/relationships model real attack paths?
- *Reason:* what questions do real engineers ask, in what order?
- *Act:* what does a correct remediation look like, and what must never be automated?

The LLM supplies general reasoning; the domain expert supplies judgment about what to reason over. This is the formula:

```
Domain expertise + software engineering + data collection
+ data modeling + LLM reasoning + automation + UX
= modern AI startup
```

## 10. The realistic solo path — service first, product second

The architecture in §2 is what a funded team builds *after* validating demand. Building it first, alone, is the classic failure mode (a year of engineering, zero customers). The proven solo sequence:

1. **Sell the outcome as a service.** E.g., "ransomware-readiness assessment + cyber-insurance questionnaire prep for SMBs," "firewall & M365 config review." Manual delivery, LLMs as *your internal leverage* — you are the orchestrator.
2. **Do it 5–10 times.** You learn which data matters, which findings customers pay for, which steps repeat. This is customer discovery *and* the domain-modeling research from §3.2, paid for by clients.
3. **Automate the repeated parts.** Collector scripts for the data you always pull, prompt templates for the analyses you always run, report generation. The service gets faster and margins improve.
4. **The product emerges from the service.** Continuous monitoring version of the assessment, self-serve report portal, etc. — now built with certainty about what customers value.

MVP tech stack when you get there: Python + FastAPI, PostgreSQL (+ pgvector), one LLM API with a fallback, a simple eval script, and a boring frontend. Add Neo4j/Elasticsearch/queues only when a real workload demands them.

**Progression of trust:** report-only → suggest actions → gated actions (human approves) → autonomous for narrow, proven action types. Each step is earned with eval data, not vibes.

## 11. Learning roadmap (maps to [[Master Index — Technology Vault]])

1. **API loop:** direct SDK calls, structured JSON output, Pydantic validation.
2. **RAG over this vault:** embed → pgvector/Chroma → retrieve → grounded answers with citations (already on your project roadmap in [[14 AI]]).
3. **Tool-calling agent:** 2–3 tools (e.g., `nmap_scan`, `check_dns`, `get_config`) in an observe→think→act loop with approval gates.
4. **Mini collector:** pull real data (M365 audit logs, AWS config, or firewall exports) into Postgres on a schedule.
5. **Eval harness:** 50 golden cases for one task (e.g., "classify finding severity"); measure before/after every prompt change.
6. **End-to-end slice:** one narrow workflow through all four stages — collect a config, organize into tables, reason (rank issues), act (generate the report). That slice *is* a sellable assessment.

Related: [[14 AI|domain overview]] · [[AI Foundations]] · [[LLMs & Prompting]] · [[Machine Learning & Deep Learning]] · [[08 Databases]] · [[12 Distributed Systems]] · [[Internet & Application Security|AI & ML Security]] · [[06 Networking]]
