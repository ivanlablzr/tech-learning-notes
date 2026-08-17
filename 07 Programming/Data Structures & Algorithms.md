---
type: note
tags: [programming, data-structures, algorithms]
---

How to organize data and compute on it efficiently — the cost model (**Big-O**), the core structures, the algorithm families, and how to choose. This is the "efficient" pillar of [[Programming Foundations]] §5, and it's not abstract theory: routing tables, DB indexes, schedulers, and caches — half this vault — *are* these structures.

## 1. Big-O — the cost model

Big-O describes how cost **grows with input size n** (usually worst case), ignoring constants — it answers "what happens when this 100-row table becomes 100 million?"

| Class | Name | Feel at n = 1M | Canonical example |
|---|---|---|---|
| O(1) | constant | instant | hash table lookup, array index |
| O(log n) | logarithmic | ~20 steps | binary search, balanced-tree ops |
| O(n) | linear | 1M steps | scan a list |
| O(n log n) | linearithmic | ~20M — fine | good sorting |
| O(n²) | quadratic | 10¹² — minutes/hours | nested loop over the same data |
| O(2ⁿ) | exponential | forget it | trying all subsets |

Rules of thumb: drop constants and lower terms (O(2n+10) = O(n)); nested loop over the same input = O(n²) — the most common accidental slowdown (e.g. `for x in list: if x in other_list` — make `other_list` a set → O(n)). Also track **space** complexity, and know **amortized** cost (a dynamic array's append is O(1) *on average* despite occasional O(n) resizes). Constants still matter in practice — cache-friendly O(n) can beat cache-hostile O(log n) at real sizes (see mechanical sympathy, [[Programming Foundations]] §5).

## 2. Core data structures

| Structure | Key ops (typical) | Use when | In the wild |
|---|---|---|---|
| **Array / dynamic array** (list, vector) | index O(1), append amortized O(1), insert-middle O(n) | default ordered collection | everything; packet buffers |
| **Linked list** | insert/delete at known node O(1), search O(n) | rarely best (pointer-chasing = cache-hostile) | LRU cache (paired with hash map), kernel queues |
| **Stack** (LIFO) | push/pop O(1) | undo, parsing/matching brackets, DFS | the call stack itself |
| **Queue / deque** (FIFO) | enqueue/dequeue O(1) | buffering, BFS, task scheduling | print/task queues → [[12 Distributed Systems|message queues]] at scale |
| **Hash table** (dict, set) | insert/lookup/delete **O(1) avg** | key→value, membership, dedup, counting — *the* workhorse | ARP tables, NAT tables, DNS caches, DB hash indexes |
| **Binary search tree** | all ops O(log n) *if balanced* | ordered data: range queries, min/max, sorted iteration | — |
| **Balanced BST** (red-black, AVL) | guaranteed O(log n) | when you need order + guarantees | language sorted maps (C++ `map`, Java `TreeMap`) |
| **B-tree / B+tree** | O(log n), wide nodes | ordered data **on disk** (minimize block reads) | **database indexes** ([[08 Databases]]), filesystems |
| **Heap** (priority queue) | peek-min O(1), push/pop O(log n) | always need the smallest/largest next | schedulers, Dijkstra, top-K, timers |
| **Graph** (adjacency list) | traverse O(V+E) | entities + relationships | networks, dependency graphs, **attack paths** (Neo4j in [[AI Products & Startup Engineering]] §3.2) |
| **Trie** (prefix tree) | ops O(key length) | prefix matching | autocomplete; **longest-prefix match = IP routing tables** ([[05 Networking]]) |
| **Bloom filter** | O(1), probabilistic ("maybe present / definitely not") | huge sets, tiny memory, false positives OK | DB/cache "is it worth looking?" pre-checks |

Security aside: hash tables degrade to O(n) under adversarial collisions — **hash-flooding DoS** — which is why languages randomize hash seeds. Data-structure choice is occasionally a security decision.

## 3. Algorithm families

- **Sorting** — know the trade-offs, then call `sort()`: **mergesort** O(n log n) always, stable, needs O(n) space; **quicksort** fastest in practice, in-place, O(n²) worst case; **heapsort** guaranteed O(n log n) in-place, slower constants. Real stdlibs use hybrids (**Timsort**). Non-comparison sorts (counting/radix) hit O(n) when keys are small integers.
- **Searching** — linear O(n) on anything; **binary search** O(log n) but *requires sorted data* (the classic bug: off-by-one on boundaries); hashing turns search into O(1) when order doesn't matter. "Sort once, search many times" is a strategy.
- **Recursion & divide-and-conquer** — base case + self-call on smaller input; each call costs stack (see [[Programming Foundations]] §2 memory). Divide-and-conquer (split → solve halves → combine) yields mergesort, quicksort, binary search.
- **Dynamic programming** — recursion + **remembering answers** to overlapping subproblems, collapsing exponential to polynomial. Top-down = recursion + memo cache; bottom-up = fill a table. Classics: Fibonacci, knapsack, **edit distance** (how `diff`, spellcheck, and fuzzy matching work).
- **Greedy** — always take the locally best option; only correct when the problem has the greedy property (proof needed, intuition lies). Wins: Dijkstra, Huffman coding (compression), interval scheduling.
- **Graph algorithms** — the family closest to your world:
  - **BFS** (queue): shortest path in *unweighted* graphs, level-by-level spread — "minimum hops," blast-radius analysis.
  - **DFS** (stack/recursion): reachability, **cycle detection**, **topological sort** (ordering with dependencies — build systems, `terraform` graphs, package managers).
  - **Dijkstra** (heap): shortest path with weights — *literally what OSPF link-state routing computes* ([[05 Networking]]).
  - **Bellman-Ford**: slower, handles negative weights — the distance-vector idea behind RIP.
  - **A\***: Dijkstra + heuristic (pathfinding in games/maps).
  - **MST** (Kruskal/Prim): cheapest set of edges connecting everything — physical network design.

## 4. Choosing — operations first

State the operations you need **before** picking the structure; choose whatever makes the hot operation cheap:

| You need | Reach for |
|---|---|
| key → value, membership, dedup | hash table |
| ordered data, range queries ("between X and Y") | balanced tree (B-tree if on disk) |
| always the next smallest/largest/soonest | heap |
| relationships, reachability, paths | graph + BFS/DFS/Dijkstra |
| prefix/longest-match | trie |
| plain ordered collection | dynamic array |

Recurring technique patterns (they solve most interview and real problems): **hash to trade space for time** (the #1 optimization — turn O(n²) lookups into O(n)); **two pointers** / **sliding window** over sorted or sequential data; **sort first** to unlock binary search, dedup, or two-pointer scans; **binary-search the answer** when you can test "is X feasible?" cheaply.

## 5. Practicing

Implement once, use stdlib forever: build a hash map, a BST, and a heap from scratch (already the L-project in [[07 Programming]]) so the complexity table above is *felt*, not memorized — then use `dict`/`set`/`heapq`/`collections` like everyone else. For problem practice, pattern-recognition beats volume: ~50–100 curated problems grouped by the §4 patterns (arrays/hashing → two pointers → sliding window → trees → graphs → DP) does more than 500 random ones.

Related: [[Programming Foundations]] · [[07 Programming|domain overview]] · [[05 Networking]] · [[08 Databases]] · [[12 Distributed Systems]] · [[Software Engineering]]
