---
type: note
tags: [ai, foundations]
---


What AI is, the spectrum of its ambition (narrow → general → super), where it's actually deployed, and the alignment problem that hangs over the long term.

## Types of AI by capability

| Type | Breadth | Status |
|---|---|---|
| **Narrow (Weak) AI** | One task/domain | **The only AI that exists today** |
| **General AI (AGI)** | Any intellectual task a human can do | Hypothetical; actively pursued |
| **Superintelligence (ASI)** | Surpasses all human intelligence | Theoretical |

**Narrow AI** is task-specific and doesn't transfer between domains (AlphaGo can't play chess), yet can vastly exceed humans within its domain — "weak" refers to *breadth*, not *quality*. Everything deployed today is narrow: LLMs, self-driving, recommendation engines, medical imaging. Limits: no transfer learning, brittle outside the training distribution, no causal understanding, can't set its own goals, data-hungry.

> A model type taxonomy: neural networks, decision trees, SVMs, learning agents — all evaluated with cross-validation, hyperparameter tuning, performance metrics, and the **bias–variance trade-off** (see [[Machine Learning & Deep Learning]]).

## Where AI is used (and how mature)

AI is a **general-purpose technology** — like electricity, it touches every domain. Major areas, each driven by the techniques in [[Machine Learning & Deep Learning]] and [[LLMs & Prompting]]:

- **Computer vision** — classification, detection (YOLO), segmentation, OCR, medical imaging, autonomous-vehicle perception.
- **NLP** — translation, summarization, QA, **code generation** (Copilot/Claude), semantic search & RAG.
- **Speech** — ASR (Whisper), TTS (ElevenLabs), voice assistants; **voice cloning** carries deepfake risk.
- **Robotics / embodied AI** — industrial arms, cobots, warehouse AMRs, humanoids; vision-language-action models (RT-2).
- **Games** — AlphaGo/AlphaZero, AlphaStar, OpenAI Five (and the proving ground for RL).
- **Healthcare** — **AlphaFold** (protein structure), clinical decision support, genomics.
- **Finance** — algorithmic trading, real-time fraud detection, credit scoring (with bias concerns).
- **Cybersecurity** — *defensive*: UEBA, malware/phishing classifiers, anomaly detection; *offensive*: AI spear-phishing, deepfake fraud, adversarial evasion (see [[Internet & Application Security|AI & ML Security]]).
- **Generative** — images (DALL·E/Midjourney/Stable Diffusion), video (Sora), music (Suno), writing/code assistants.
- **Scientific discovery** — weather (GraphCast), materials, AlphaTensor; AlphaFold & Hopfield won the 2024 science Nobels.

| Maturity | Domains |
|---|---|
| **Production** | Vision, NLP/LLMs, speech, recommendation, fraud, healthcare imaging |
| **Early production** | Drug discovery, generative video, L2/L3 autonomous driving |
| **Research** | General robotics, L4/L5 driving, scientific paradigm shifts |

## AGI — and why it's hard

AGI would do **domain-general learning** (transfer skills anywhere), commonsense and **causal** reasoning (interventions/counterfactuals — Pearl's ladder), self-directed goals, and sample-efficient learning. Open problems: **symbol grounding** (tokens with no grounded meaning), causation vs correlation, **sample efficiency** (humans learn from few examples), **catastrophic forgetting** (continual learning), and brittleness under **distribution shift**.

Competing bets on the path: the **scaling hypothesis** (more compute → emergent capability — Sutskever, Amodei), **world models** (LeCun's JEPA — predict future states for planning), **neurosymbolic** (neural perception + symbolic logic — Marcus), and **RL + LLM agents** (tool-using, goal-directed). Timelines range from "a few years" (Altman/Hassabis/Amodei) to "decades, needs new architecture" (LeCun) to "current approach can't get there" (Marcus) — reflecting genuine uncertainty about *what AGI even means*.

## The alignment problem (and superintelligence)

If a system optimizes a **misspecified** objective with enough capability, it can be catastrophic — the **paperclip maximizer**, **Goodhart's Law** (a proxy metric stops measuring the goal), and **instrumental convergence** (self-preservation, resource acquisition regardless of final goal). With superintelligence (Bostrom's speed/collective/quality forms, possibly via recursive self-improvement → an **intelligence explosion**), the **control problem** is that you can't easily oversee something smarter than you.

Safety research directions: **RLHF** (train on human preferences), **Constitutional AI** (the model critiques itself against principles — Anthropic), **mechanistic interpretability** (read the model's internals), **corrigibility** (want to be corrected), and **capability containment**. The field is split — cautious (Bostrom, Russell, Hinton, Yudkowsky) vs skeptical-of-existential-risk (LeCun, Ng) — with regulation emerging (EU AI Act, AI Safety Institutes, frontier-model evaluations).

## Open problems beyond alignment

Alignment is the deepest long-term concern, but current systems have unsolved problems that shape what they can responsibly be used for today:

- **Reliability & hallucination** — models generate confidently even when wrong and have no built-in "I don't know," which blocks unsupervised use in medicine, law, and finance (mitigations — RAG, tool use, grounding — in [[LLMs & Prompting]]).
- **Reasoning & planning** — strong local coherence, weak deep multi-step logic and long-horizon planning under many constraints (the same gap that makes AGI hard, above).
- **Interpretability** — it's hard to explain *why* a model produced an answer, which hurts debugging, regulation, and trust in high-stakes domains → mechanistic interpretability research (above).
- **Data provenance & copyright** — training corpora scraped from the web raise unresolved copyright, consent, and fair-use questions; responses include licensed datasets, synthetic data, and opt-out mechanisms.
- **Efficiency & cost** — frontier models are expensive to train and serve; shrinking them (quantization, distillation) to run cheaply on consumer hardware is an active engineering frontier (see [[Machine Learning & Deep Learning]], [[LLMs & Prompting]]).
- **Personalization vs privacy** — adapting to a user's style and knowledge without harvesting their data; explored via federated learning, on-device models, and differential privacy.

These are engineering-and-governance problems (tractable, incrementally improving) — distinct from the alignment problem above, which is about *control* as capability grows.

Related: [[Machine Learning & Deep Learning]] · [[LLMs & Prompting]] · [[14 AI|domain overview]] · [[Internet & Application Security|AI & ML Security]]
