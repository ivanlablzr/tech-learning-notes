---
type: reference
tags: [programming, resources, learning, projects]
domains: [programming, software-engineering]
maturity: growing
updated: 2026-07-20
---

> [!abstract] What this note is
> The **single home for how to *get better* at programming** — roadmaps, platforms, courses, books, competitive programming, and project ideas. The concept notes ([[Programming Foundations]], [[Data Structures & Algorithms]], [[Software Engineering]]) hold the *knowledge*; this holds the *routes to acquire it*.

> [!tip] Read this first — the meta-strategy
> Your own [[07 Programming|domain note]] and [[Personal weaknesses]] already say it: **you learn to code by building, not watching, and you pick ONE path and finish it.** Programming is your stated weak area, so the failure mode to avoid is *tutorial-hopping* — collecting resources instead of finishing one. This note is deliberately long so you can *choose once*, not so you do everything. Treat it as a menu, not a checklist.
>
> The three things below train **different skills** — don't confuse them:
> - **Fluency** (write code without fighting syntax) → small daily reps: Exercism, Codewars.
> - **Problem-solving / "thinking like a programmer"** → build real things + read others' code + one structured curriculum. *This is the big one, and puzzles alone won't get you there.*
> - **Interview / competitive** → LeetCode, Codeforces. Useful, but **not** the same as software engineering — see §5's warning.

---

## 1. Roadmaps — the map before the journey

Use these to *orient*, then close them. A roadmap is a planning tool, not a study plan — don't try to "complete" one.

| Resource | What it is | Best for |
|---|---|---|
| **[roadmap.sh](https://roadmap.sh)** ⭐ | The canonical interactive dev roadmaps (backend, DevOps, Python, cybersecurity…) | Seeing the whole territory and what depends on what |
| **[awesome-roadmaps](https://github.com/orsanawwad/awesome-roadmaps)** | Curated index *of* roadmaps (the repo you linked) | A catalogue when roadmap.sh doesn't cover a niche |
| **[Teach Yourself CS](https://teachyourselfcs.com)** ⭐ | Opinionated "the 9 subjects + one great book/course each" | The **CS foundations** behind the code — your best single filter for what actually matters |
| **[OSSU Computer Science](https://github.com/ossu/computer-science)** | A full free university CS curriculum, ordered | If you ever want a complete degree-equivalent path |

> [!note] How these map to your vault
> roadmap.sh's ordering is basically the dependency graph you already built in [[Master Index — Technology Vault]]. Teach Yourself CS is the highest-signal of the four — it's a *curation*, not a dump, which is exactly what a weak-area learner needs.

---

## 2. Problem-solving platforms — daily fluency & thinking

The core of "solve problems like a programmer." Do a little, often. **Exercism is the standout** because of human mentoring.

| Platform | Free? | Best for | Note |
|---|---|---|---|
| **[Exercism](https://exercism.org)** ⭐ | Free | Learning idioms in a new language + **free human mentoring** on your solutions | The mentoring is the differentiator — nowhere else gives feedback on *how you think*, free. Start here. |
| **[boot.dev](https://boot.dev)** ⭐ | Paid (~$348/yr) | Structured backend path (Python→Go), gamified, project-based | *You already use it — finish it before adding anything.* |
| **[Codewars](https://codewars.com)** | Free | Bite-sized "katas" ranked by difficulty; see others' solutions after | Best *after* solving: read the top-voted solutions to learn better approaches |
| **[Advent of Code](https://adventofcode.com)** ⭐ | Free | Yearly December puzzles, story-driven, all languages | The most *fun* way to build fluency; do past years anytime |
| **[CodinGame](https://codingame.com)** | Free | Solving problems by writing bots for games | Good if gamification keeps you going |
| **[Codédex](https://codedex.io)** | Freemium | Beginner, RPG-style intro | Only if you want the gentlest possible on-ramp |

> [!warning] Better-alternative honesty
> **Edabit** (which you'll see recommended) has declined — Exercism + Codewars cover the same ground better and are actively maintained. And don't grind *any* of these as a substitute for building. Two katas as a warm-up, then go work on a real project. Puzzles build fluency; they do **not** teach architecture, debugging real systems, or reading a large codebase — which is 90% of the job.

---

## 3. Core curricula — pick ONE, finish it

These are the "structured path" leg. Overlaps with your [[IT Certifications and Learning Resources per Domain#Programming & Software Engineering|certs note]]; kept here for completeness.

| Course | Free? | Best for |
|---|---|---|
| **[CS50x](https://cs50.harvard.edu/x/)** (Harvard) ⭐ | Free | The reality *under* the code — C, memory, then Python/SQL/JS. Best mental-model builder |
| **[The Odin Project](https://theodinproject.com)** | Free | 100% project-based full-stack; forces you to research like a real dev |
| **[Full Stack Open](https://fullstackopen.com)** (Helsinki) | Free | Modern web (React/Node/GraphQL), rigorous |
| **[MOOC.fi Python / Java](https://programming-mooc.org)** (Helsinki) ⭐ | Free | The best free *first* programming course, ordered and thorough |
| **[MIT Missing Semester](https://missing.cs.stanford.edu)** ⭐ | Free | Shell, git, vim, debugging, tooling — the stuff courses skip. Short, essential |

> Pick **one** primary (CS50x if you want depth, MOOC.fi if you want a gentle complete intro), and run Missing Semester alongside — it's only ~10 hours and makes everything else easier.

---

## 4. Build-your-own — "understand by rebuilding" ⭐

This is where "think like a programmer" actually happens: rebuild real tools and read others' code. **The highest-leverage section for you.**

| Resource | Free? | Best for |
|---|---|---|
| **[Codecrafters](https://codecrafters.io)** ⭐ | Freemium | "Build your own **Git / Redis / HTTP server / SQLite / shell / DNS server / interpreter**" — guided, hard, in *your* language |
| **[Build Your Own X](https://github.com/codecrafters-io/build-your-own-x)** | Free | The index of "build your own database/docker/git/language/…" tutorials |
| **[Crafting Interpreters](https://craftinginterpreters.com)** ⭐ | Free online | Build a full programming language *twice*. One of the best books on how code really works |
| **[Nand2Tetris](https://www.nand2tetris.org)** ⭐ | Free | Build a whole computer from NAND gates → assembler → VM → language → Tetris. Bridges to your [[02 Electronics|Electronics]] |
| **[Fly.io — Gossip Glomers](https://fly.io/dist-sys/)** | Free | Distributed-systems challenges — a bridge to [[12 Distributed Systems]] |

> [!tip] The cyber crossover
> A "build your own X" that doubles as security skill: write your own **port scanner, packet sniffer (Scapy), log parser, or a small C2 stub in Go/Rust**. That feeds [[Cybersecurity Skills Roadmap]] and your [[16 Home Lab Projects|home lab]] at the same time — programming practice *and* portfolio for your red-team track.

---

## 5. Competitive programming — powerful, but know what it's for

| Resource | Free? | Best for |
|---|---|---|
| **[CSES Problem Set](https://cses.fi/problemset/)** ⭐ | Free | The best-structured intro to competitive algorithms |
| **[Codeforces](https://codeforces.com)** | Free | Live contests, huge community, the standard CP arena |
| **[AtCoder](https://atcoder.jp)** | Free | Cleaner beginner contests (ABC series) |
| **[USACO Guide](https://usaco.guide)** | Free | Best free *structured* CP curriculum |
| **[Project Euler](https://projecteuler.net)** | Free | Math-heavy problems — pairs with your [[00 Mathematics|maths]] gap |
| **[LeetCode](https://leetcode.com)** | Freemium | **Interview** prep specifically (patterns via **NeetCode**) |

> [!warning] The trap to avoid
> Competitive programming and interview grinding sharpen **algorithmic problem-solving**, and they *feel* like progress because they're measurable. But they are **not** software engineering — they teach nothing about architecture, testing, maintainability, reading large codebases, or shipping. Elite competitive programmers can be poor engineers and vice-versa. Given your goal is *building and shipping products*, treat CP as **cross-training, not the main sport**: a few problems a week for sharpness, while the real work is §4 (building) and §6 (reading). Do LeetCode **only** when you have interviews coming up.

---

## 6. Books — by level

Buy few, finish them. (~$25–60 each unless noted.)

**Foundations & thinking**
- **"Code"** (Petzold) — software↔hardware from first principles
- **[SICP](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html)** (free) — *how to think*; hard but transformative
- **"The Pragmatic Programmer"** (Hunt & Thomas) — how pros actually work

**Craft**
- **"Clean Code"** (Martin) — read *critically*, it's dogmatic
- **"A Philosophy of Software Design"** (Ousterhout) ⭐ — the better, shorter counterpoint to Clean Code
- **"The Missing README"** — what CS degrees don't teach about working as an engineer

**Algorithms**
- **"Grokking Algorithms"** (gentle, illustrated) → **CLRS** (reference, not cover-to-cover)

**Systems & scale (later)**
- **"Designing Data-Intensive Applications"** (Kleppmann) ⭐ — the modern classic; bridges to [[08 Databases]] & [[12 Distributed Systems]]

**Language-specific**
- Python: **"Automate the Boring Stuff"** (free) → **"Fluent Python"** (advanced)
- C: **K&R "The C Programming Language"** — for systems/memory feel
- Go: **"The Go Programming Language"** (Donovan & Kernighan)

---

## 7. Curated reference lists — the "everything" repos

Bookmark, don't read end-to-end. Mine them when you hit a specific gap.

- **[every-programmer-should-know](https://github.com/mtdvio/every-programmer-should-know)** — the repo you linked; concepts every dev should meet at least once
- **[Awesome lists](https://github.com/sindresorhus/awesome)** — the index of curated lists for *any* topic/language
- **[The Missing Semester](https://missing.cs.stanford.edu)** — (also in §3) tooling reference
- **[Papers We Love](https://github.com/papers-we-love/papers-we-love)** — when you want the primary CS literature (ties to your R&D goals)

---

## 8. Project ideas — the point of all the above

*Replaces the dangling `[[Coding Projects]]` link. Pull from here into your [[Daily-note-template|daily note]] ⭐ or [[Mission Priority]]. A project isn't done until it has a README and a writeup — the writeup is the portfolio.*

**Tier 1 — fluency (days)**
- [ ] CLI to-do / notes app **with tests** — your first "real" repo
- [ ] Password generator + strength checker (feeds cyber)
- [ ] A script that automates one annoying task in your actual life

**Tier 2 — applied (weeks)**
- [ ] **Port scanner** in Python → then rewrite in Go (feel the difference) — feeds [[Cybersecurity Skills Roadmap]]
- [ ] Personal dashboard pulling an API (weather, your finances) — auth + DB + deploy
- [ ] Log parser / small SIEM-style tool for your [[16 Home Lab Projects|home lab]]
- [ ] Implement a **hash map** and a **B-tree** from scratch — [[Data Structures & Algorithms]] made real

**Tier 3 — understand-by-rebuilding (the L6 anchor)**
- [ ] Codecrafters: **build your own Redis / HTTP server / DNS server**
- [ ] A REST **and** gRPC service exposing the *same* logic (the API-contracts gap your [[07 Programming]] note flags as owned-but-thin)
- [ ] Crafting Interpreters: **build a language**
- [ ] A small **C2 / agent** in Go or Rust — programming + red-team portfolio in one

**Tier 4 — venture seeds (ties to [[Master Index — Technology Vault]] §8)**
- [ ] An AI-security tool: prompt-injection scanner, or a RAG pipeline with adversarial tests
- [ ] Anything from your startup-ideas list that needs a working prototype

---

## Connections

- [[07 Programming]] — the domain map this note serves; **why** each skill matters
- [[Programming Foundations]] · [[Data Structures & Algorithms]] · [[Software Engineering]] — the concept notes; this note is *how* to learn them
- [[IT Certifications and Learning Resources per Domain]] — overlapping resources framed around certs/career
- [[Cybersecurity Skills Roadmap]] · [[16 Home Lab Projects]] — where the security-flavoured projects feed
- [[Learning Efficiency]] — *how to learn* (spacing, retrieval) applied to all of the above
- [[00 Mathematics]] — the prerequisite gap behind algorithms & competitive programming

## Related

[[Master Index — Technology Vault]]
