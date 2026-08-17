---
type: note
tags: [programming, fundamentals, craft]
---

What programming actually is, how source code becomes execution, and the craft trilogy — writing code that is **correct**, **efficient**, and **secure** — plus concurrency, debugging, and how programs are structured into systems.

## 1. What programming actually is

Every program, from a shell script to an OS kernel, is the same thing: **state + behavior**. Data (state) is transformed by instructions (behavior) — `input → transform → output`, with conditionals to branch and loops to repeat. Languages are just different notations for this.

The actual skill of programming is **abstraction**: wrapping a bundle of complexity in a name (function → class → module → service) so you can think one level up without holding the details in your head. Decomposition — splitting a problem until each piece is trivially solvable — is abstraction in reverse. This is why programming knowledge transfers across languages: syntax is the cheap part; modeling problems is the expensive part.

The second durable skill: reading **documentation as the source of truth** — official docs, RFCs, man pages — to solve problems autonomously rather than pattern-matching from tutorials.

## 2. How code runs — language machinery

What happens between your `.py`/`.rs` file and electrons moving:

| Model | How | Examples | Trade-off |
|---|---|---|---|
| **Compiled (AOT)** | source → machine code before running | C, C++, Rust, Go | fastest execution; per-platform binaries; compile step |
| **Interpreted** | a VM executes bytecode line-by-line at runtime | Python, Ruby, Bash | instant iteration, portable; ~10–100× slower |
| **JIT** | interpret first, compile hot paths at runtime | JavaScript (V8), Java (JVM), C# | near-compiled speed after warm-up |

**Type systems** — *when* are type errors caught, and *how strictly*:
- **Static** (C, Rust, Go, TypeScript): checked at compile time — types are machine-verified documentation; whole bug classes never reach runtime.
- **Dynamic** (Python, JS): checked at runtime — faster to start, errors surface in production. Retrofits (**mypy**, TypeScript) exist because dynamic codebases hurt at scale.
- **Strong vs weak**: does the language silently coerce types? (`"1" + 1` errors in Python, gives `"11"` in JS — a whole category of JS bugs).

**Memory management** — where correctness, performance, and security meet:
- **Stack**: automatic, fast, scoped to the function call. **Heap**: dynamic, lives until freed.
- **Manual** (C/C++): you `malloc`/`free` — source of leaks, use-after-free, buffer overflows, i.e. most memory-corruption CVEs in history.
- **Garbage collected** (Python, Go, Java, JS): the runtime frees unreachable memory — safe, costs CPU and pause times.
- **Ownership** (Rust): the compiler proves at compile time when memory can be freed — memory safety *without* GC, which is why Rust is eating systems/security programming.

## 3. Paradigms — what each optimizes for

Modern languages are multi-paradigm; you pick the style per problem, not per religion.

| Paradigm | Core idea | Shines at | Watch out |
|---|---|---|---|
| **Imperative / procedural** | explicit steps mutating state | scripts, algorithms, systems code | state sprawl as programs grow |
| **OOP** | bundle state + behavior into objects (encapsulation, polymorphism, inheritance) | modeling domains with many kinds of *things* | deep inheritance trees — prefer **composition** |
| **Functional** | pure functions (no side effects), immutability, higher-order functions (`map`/`filter`/`reduce`) | data transformation pipelines, anything concurrent (nothing shared = nothing to race) | performance of naive copies; learning curve |
| **Declarative** | describe *what*, engine decides *how* | SQL, HTML/CSS, Terraform, Kubernetes manifests | debugging the engine's "how" when it misbehaves |
| **Event-driven** | react to events via handlers/callbacks | GUIs, network servers, embedded ISRs ([[07 Programming]] §embedded) | callback spaghetti — tamed by async/await |

The pragmatic blend in most real code: imperative core, objects for domain modeling, functional style for data flow, declarative for infra and queries.

## 4. Writing correct code

Correct = matches intent for **all** inputs — the happy path is the easy 20%.

- **Make invalid states unrepresentable.** Use types/enums instead of strings and booleans (`Status.ACTIVE`, not `"active"`); if the invalid combination can't be constructed, it can't be a bug.
- **Validate at trust boundaries, fail fast.** Check inputs where data enters (API handler, file parser, user input), then trust it inside. An early crash with a clear error beats silent corruption ten functions later.
- **Errors are part of the contract.** Decide the strategy: exceptions (Python — catch specific ones, never bare `except`) vs explicit results (Go's `err != nil`, Rust's `Result`). Never swallow an error you can't handle; propagate with context.
- **Keep functions small and single-purpose** (~one screen): testable, nameable, reviewable. Bound every loop; declare data in the smallest scope; **check every return value**; compile/lint with zero warnings; assert your assumptions where they'd fail silently. *(The distilled essence of safety-critical coding practice.)*
- **Tests are the executable specification** — a function without a test is a claim without evidence. Pyramid, TDD, and doubles: [[Software Engineering]] §4.
- **Prefer boring code.** Cleverness optimizes writing; software spends 90% of its life being *read* and *modified*.

## 5. Writing efficient code

- **The optimization hierarchy** — improvements in descending order of magnitude:
  `better algorithm (O(n²)→O(n log n)) ≫ better data structure ≫ better code ≫ micro-optimizations`
  A day in [[Data Structures & Algorithms]] is worth a year of code tweaking.
- **Measure, don't guess.** The bottleneck is almost never where you think. Profile first (§8) and optimize only what's proven hot. "Premature optimization is the root of all evil" — but so is *premature pessimization* (choosing O(n²) when O(n) is equally easy).
- **Mechanical sympathy** — know the latency ladder: CPU cache (ns) < RAM (100 ns) < SSD (100 µs) < network (ms). Consequences: contiguous arrays beat pointer-chasing structures in practice (cache locality); **I/O dominates CPU** — batch it, buffer it, cache it; **network calls dominate everything** — every round trip you eliminate outweighs a thousand code tweaks. This is the same lesson at every scale, up to [[12 Distributed Systems]].
- **Caching** is the universal speed trick and the source of the two hard problems: invalidation and naming. Cache when reads ≫ writes and staleness is tolerable.

## 6. Writing secure code

The attacker's view: your program is a machine that processes *their* input. (SDLC-level practice in [[Software Engineering]] §7; the full threat landscape in [[Internet & Application Security]].)

- **All input is hostile until validated** — from users, files, networks, *and other services*. The injection family (SQLi, command injection, path traversal, XSS) is one bug repeated: attacker data interpreted as code. Fix is structural, not sanitization-by-regex: **parameterized queries**, no shell string concatenation (`subprocess` with arg lists), canonicalize paths, escape output for its context.
- **Allowlist over blocklist.** Enumerate what's valid; rejecting known-bad always misses unknown-bad.
- **Least privilege for code**: run as non-admin, scoped API tokens, minimal file/network permissions — a compromised process gets only what it had (same principle applied to LLM agents in [[LLMs & Prompting]] §8).
- **Secrets never live in source**: env vars → secret stores; `.gitignore` before first commit (git history is forever); rotate on any leak.
- **Dependencies are your attack surface**: pin versions, scan (`pip-audit`, Dependabot), beware typosquats — you ship every line of every package you import.
- **Don't roll your own crypto** — use vetted libraries and boring standards ([[Cryptography]]).
- **Fail closed** (error → deny, not allow) and **log security-relevant events** (auth failures, permission denials) — future-you in incident response will thank present-you.
- Memory-unsafe languages (C/C++) add the overflow/UAF class — §2's argument for Rust/Go in new security-sensitive code.

## 7. Concurrency & parallelism

**Concurrency** = structuring a program to *deal with* many things at once; **parallelism** = literally *doing* many at once on multiple cores. A single-core server handling 10K connections is concurrent, not parallel.

| Model | Memory | Overhead | Best for |
|---|---|---|---|
| **Processes** | isolated | heavy | CPU-bound parallel work; fault isolation |
| **Threads** | shared | medium | parallel work sharing state (careful) |
| **Async/await** | shared, single thread | minimal | massive I/O concurrency (10K sockets) |

Python note: the **GIL** means threads don't parallelize CPU work — threads/async for I/O-bound, `multiprocessing` for CPU-bound.

- **The hazards:** **race conditions** (two writers, interleaved — the check-then-act bug), **deadlock** (A holds lock 1 wants 2, B holds 2 wants 1 — prevent with global lock ordering), starvation. Synchronization tools: mutex/lock, semaphore, atomic operations.
- **The safer models:** don't share — **message passing** via channels (Go: "share memory by communicating, don't communicate by sharing memory"), actor model, or immutability (nothing mutable = nothing to race — the functional paradigm's payoff).
- This is a bridge: the primitives are [[04 Operating Systems|OS scheduling/threads]]; the same problems reappear with partial failure and no shared clock in [[12 Distributed Systems]].

## 8. Debugging & profiling

**Debugging is the scientific method**: reproduce reliably → read the error *bottom-up* (the first stack frame in *your* code) → form one hypothesis → test it with minimal change → fix → **add the regression test** that would have caught it.

- **Tool ladder:** print/logging (fine, but leave *structured logging* behind, not `print("here3")`) → **debugger** (breakpoints, step, inspect state — pdb/VS Code/gdb) → `git bisect` (binary-search history for the breaking commit) → memory tools (Valgrind/ASan for C/C++).
- **Techniques:** minimal reproduction (delete everything that doesn't affect the bug — often reveals it by itself); rubber-duck explanation; change one variable at a time; suspect your own code first, the library second, the compiler ~never.
- **Profiling** (the "measure" of §5): CPU profilers (sampling → **flamegraphs**; `cProfile`/py-spy, `perf`), memory profilers (leaks, allocation churn), I/O tracing. Read a flamegraph: wide = expensive, that's your target.

## 9. From programs to systems

The first architectural question: **who owns the data and has authority?**

- **Centralized** (client-server): the server is the authority — simple to manage and secure; single point of failure and scaling bottleneck.
- **Decentralized / P2P**: every node both client and server — no central authority, scales with peers, harder to secure (no central policy). Discovery via broadcast/DHT/bootstrap; **hybrid** (central tracker, P2P transfer) and **federated** (interoperating servers — email, Mastodon, Matrix) blend the two. Uses: BitTorrent, blockchains, Tor, WebRTC. Threats: Sybil attacks, DHT poisoning, malware distribution.
- **Application shape:** **monolith** (one deployable — start here) → **microservices** (independent services owning one capability each, talking over APIs) — buys independent scaling/deployment at the price of distributed-systems complexity. Split only when team size or scaling forces it. Deep dives: [[12 Distributed Systems]], [[13 Architecture]].

Related: [[Data Structures & Algorithms]] · [[Software Engineering]] · [[Languages & Applied Programming]] · [[07 Programming|domain overview]] · [[04 Operating Systems]] · [[12 Distributed Systems]] · [[Internet & Application Security]] · [[Cryptography]]
