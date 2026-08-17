---
type: note
tags: [ai, learning-path, resources, roadmap]
---

A deep learning path for someone with a **networking + cybersecurity** background who wants more than "prompt engineering in 30 minutes" — the goal is to understand the scientific foundations, build real AI systems, evaluate others' claims critically, and eventually create an AI startup. That ambition means mastering several layers of the stack, not just being a good model *user*. Sequenced in six levels, from math to model training. (Maps to the compressed learning path in [[14 AI]] and the projects in [[AI Products & Startup Engineering]] §11.)

## Level 1 — Foundations

Goal: understand what an LLM actually *is*.

Master: probability, linear algebra, differential calculus (engineering level), Python, data structures, GPU vs CPU, CUDA (conceptually), how PyTorch works.

Read: *Mathematics for Machine Learning*; *Deep Learning* (Goodfellow — the "bible").

## Level 2 — Machine Learning

Before LLMs, understand *why* they exist — almost every LLM concept descends from classical ML (see [[Machine Learning & Deep Learning]] §1).

Learn: regression, classification, gradient descent, loss functions, overfitting, regularization, cross-validation, feature engineering.

Reference course: Andrew Ng's Machine Learning course.

## Level 3 — Deep Learning

Understand: neural networks, backpropagation, CNNs, RNNs, LSTMs, embeddings, attention (see [[Machine Learning & Deep Learning]] §2).

Then read **"Attention Is All You Need"** — arguably the most important paper of the last decade. It's hard at first; read it several times. Understanding comes in layers.

## Level 4 — LLMs

Only at this stage. Understand: tokenization, BPE, SentencePiece, embeddings, positional encoding, the Transformer, multi-head attention, KV cache, sampling, temperature, top-p, top-k (see [[LLMs & Prompting]] §1, §5).

Then: inference, training, fine-tuning, LoRA, QLoRA, quantization. After this, terms like GGUF, FP16, INT4, AWQ, GPTQ will carry precise meaning.

## Level 5 — AI systems

Where most content creators stop — and where real products begin (this is the whole subject of [[AI Products & Startup Engineering]]).

- **Agents** — why Claude Code is impressive; the observe → decide → act → observe loop.
- **MCP** — why the Model Context Protocol changes how models use tools.
- **RAG** — the difference between memory, context, vector store, and embeddings.
- **Vector databases** — cosine similarity, FAISS, HNSW, ANN indexes.
- **Tool calling** — why a model can call an API, use Git, launch Docker, or open VS Code *without* "being intelligent."
- **Context engineering** — arguably now more important than prompt engineering; the best AI products lean more on good context management than on a clever prompt.

## Level 6 — Training models

Very few people go this far. Understand: pre-training, SFT, RLHF, DPO, PPO, distillation, Mixture of Experts, synthetic data (see [[LLMs & Prompting]] §2). You'll then understand why some companies can train competitive models with fewer resources than rivals.

## Understanding GPUs

Indispensable, and often ignored — modern AI is nearly as much a hardware problem as an algorithms problem. Study: VRAM, Tensor Cores, CUDA, NCCL, and the three parallelism strategies — pipeline, tensor, and data parallelism.

## Reading papers

What separates a real expert from someone who just follows the news. Essentials: Attention Is All You Need, InstructGPT, LoRA, RLHF, DPO, Chain of Thought, Toolformer, ReAct, RETRO, Llama, DeepSeek-V3 (architecture and training). You won't understand everything at first — that's normal. Get used to reading arXiv directly rather than waiting for a summary; slow at first, but it gives a truer picture of what's actually advancing.

## Understanding the companies

To build a startup, understand the business models too. Study OpenAI, Anthropic, Google DeepMind, Meta, Mistral AI, and xAI — not just their models, but their product choices, APIs, positioning, and competitive advantages (the moat analysis lives in [[AI Products & Startup Engineering]] §8).

## Building projects

From here, spend at least half your time building. Practice reveals limits and trade-offs that courses rarely cover:

- Train a small Transformer.
- Build a chatbot with RAG.
- Develop an agent that writes code.
- Implement a memory system.
- Build a cybersecurity assistant.
- Deploy an LLM on Kubernetes.
- Experiment with several open-source models.

## Best resources

**Books:** Deep Learning · Mathematics for Machine Learning · Designing Machine Learning Systems · AI Engineering.

**Courses:** Andrew Ng · DeepLearning.AI.

**YouTube (technical explanation over announcements):** Andrej Karpathy (*Let's build GPT from scratch*) · 3Blue1Brown (math foundations) · Yannic Kilcher (paper analyses).

**Papers:** read arXiv directly rather than waiting for someone to summarize.

## The most important point

If the goal is real impact and a startup — not just being a good model user — aim to understand the **full stack**, a combination that's rare because most people master only one or two layers:

1. **Mathematics** — why the algorithms work.
2. **Research** — how new models are designed.
3. **Engineering** — how to make them reliable, fast, and scalable.
4. **Product** — how to solve a real problem for real users.
5. **Business** — why some companies build durable value while others just wrap an API.

A networking + cybersecurity background is already a solid base for growing into this full-stack view — and domain expertise is itself the differentiator ([[AI Products & Startup Engineering]] §9).

Related: [[14 AI|domain overview]] · [[AI Foundations]] · [[Machine Learning & Deep Learning]] · [[LLMs & Prompting]] · [[AI Products & Startup Engineering]] · [[Master Index — Technology Vault]]
