---
type: note
tags: [ai, llm, prompting]
---


Large language models — how they work, the model landscape, how to prompt them, how to build with them, and how they break.

## 1. How LLMs work — the Transformer

LLMs are deep models trained to predict the next **token** (subword) over massive text, built on the **Transformer** (Vaswani 2017, "Attention Is All You Need"). Capabilities like reasoning and code generation are **emergent** — they appear only at sufficient scale (billions of params, trillions of tokens).

- **Self-attention** `Attention(Q,K,V) = softmax(QKᵀ/√dₖ)·V` lets every token directly weigh every other token; **multi-head** runs it in parallel to capture different relationships. A block = attention + feed-forward, each with a residual connection and LayerNorm.
- **Positional encoding** injects order (sinusoidal → learned → **RoPE** for long contexts) since tokens are processed in parallel.
- **GPT-style models are decoder-only** with **causal masking** (each token sees only prior tokens) → autoregressive generation.
- **Context window** = max input+output tokens (GPT-4o 128K, Claude 200K, Gemini 1.5 Pro 1M).

## 2. Training

1. **Pretraining** — next-token prediction over trillions of tokens (Common Crawl, books, code) on thousands of GPUs (GPT-4 est. >$100M).
2. **SFT** — fine-tune on high-quality instruction demonstrations → follows directions.
3. **RLHF** — humans rank outputs → a **reward model** learns preferences → **PPO** optimizes the LLM toward it (with a KL penalty). Yields "helpful, harmless, honest" — but also **sycophancy**. **DPO** is a simpler reward-model-free alternative; **Constitutional AI** (Anthropic) has the model critique itself against principles.

**Efficient adaptation:** **LoRA** (train tiny low-rank matrices, ~0.1–1% of params), **QLoRA** (LoRA on a 4-bit base — fine-tune 70B on one GPU). **Quantization** (FP16→INT8→INT4) shrinks models for cheaper/faster inference (llama.cpp, GPTQ/AWQ).

## 3. The model landscape

| Model | Org | Notable |
|---|---|---|
| GPT-4o / o-series | OpenAI | multimodal; o-series = extended reasoning |
| Claude (Opus/Sonnet/Haiku) | Anthropic | strong reasoning, 200K context, Constitutional AI |
| Gemini 1.5 Pro | Google | 1M-token context, multimodal |
| Llama 3 | Meta | open weights (8B–405B) |
| Mistral / Mixtral | Mistral | efficient, open, MoE |
| DeepSeek-R1 | DeepSeek | open reasoning model (MoE) |

## 4. Capabilities & limitations

Emergent: few-shot in-context learning, chain-of-thought reasoning, code generation, instruction following, tool use. Limitations: **hallucination** (confident but false), **knowledge cutoff**, finite context, no persistent memory, stochastic outputs, multi-step-math/logic failures, training-data **bias**. **Mitigate** with **RAG** (ground answers in retrieved documents), tool use (search/calculator/code), grounding+citations, and self-critique.

## 5. Inference controls

`temperature` (0 deterministic → >1 creative — use **0 for extraction/code**), `top_p`/`top_k` (nucleus/top-k sampling), `max_tokens`, presence/frequency penalties (reduce repetition). Decoding: greedy (fast, repetitive), beam search (quality), sampling (varied).

## 6. Prompt engineering

Treat the model as a brilliant colleague who needs precise, context-rich instructions — ambiguity → mediocrity. Core moves:

- **Be specific:** task, output **format**, length, audience, tone, and what to exclude.
- **System role** sets persona/constraints separately from the user turn.
- **Few-shot:** 2–5 examples in the exact desired format (often beats describing it).
- **Chain-of-Thought:** "think step by step" — intermediate tokens act as working memory, boosting reasoning. **Self-consistency** (majority vote over samples) and **Tree of Thoughts** (explore/backtrack) extend it.
- **ReAct:** alternate Thought → Action (tool) → Observation for agents.
- **Structured output:** request strict JSON; use **XML tags** (`<context>`, `<code>`, `<task>`) to delimit sections; **prompt chaining** breaks big tasks into smaller, debuggable steps.

## 7. Building with LLMs (developer)

The goal is **reliable, parseable output**, not just good prose.

- **APIs:** OpenAI puts the system prompt as a `role:system` message; **Anthropic** passes `system=` as a top-level parameter. Pick the cheap tier (gpt-4o-mini / Haiku) for classification/extraction; reserve flagships for hard reasoning.
- **Structured output:** ask for JSON (OpenAI JSON/json_schema mode), then **validate with Pydantic** (retry on failure via `instructor`). Use `temperature=0`.
- **Function calling / agents:** define tools (name, description, JSON-schema params); the model emits a call, *you* execute it and feed the result back in a loop (observe → think → act). Frameworks: LangChain, LlamaIndex (RAG), AutoGen/CrewAI (multi-agent) — but the direct SDK is fine for simple loops.
- **RAG:** `query → embed → vector search → top-K chunks → LLM context → answer`. Chunk docs ~512–1024 tokens with overlap; store in a vector DB (Chroma/pgvector/Pinecone); retrieve by cosine similarity.
- **Production concerns:** **stream** tokens for responsive UIs; count tokens (`tiktoken`) and manage context (sliding window, hierarchical summarization); handle errors with **exponential backoff** on rate limits + model fallback; cut cost with **prompt caching** (Anthropic prefix caching / OpenAI auto-cache ~50%), the Batch API, and response caching.

## 8. LLM security

- **Prompt injection** — the signature LLM vuln: **direct** ("ignore previous instructions…") or **indirect** (malicious text in a fetched web page/PDF/email that an agent reads). Defenses: separate system from user input via API roles, input/output filtering, privilege separation, and human approval for sensitive actions.
- **Never trust LLM output for security decisions** (access control, authz) — it can be coerced to emit `{"authorized": true}`. Use deterministic logic.
- Other risks: **jailbreaking** (roleplay/obfuscation), **data poisoning** (training backdoors), **model inversion/extraction** (leak training data / clone the model). Validate/sandbox any code the model produces; log all I/O.
- **Practical AI-dev safety:** protect **API keys** (never hard-code → env var → OS keychain/secret store); **sandbox untrusted repos** in a VM (hidden prompt injection in a README/comment); pin dependencies (typosquats like `lodash-utils`); review every tool/approval action before accepting. Don't send PII/secrets to a third-party API.

> See [[Internet & Application Security|AI & ML Security]] for the broader adversarial-ML threat model.

Related: [[AI Foundations]] · [[Machine Learning & Deep Learning]] · [[14 AI|domain overview]] · [[Internet & Application Security]] · [[Cryptography]]
