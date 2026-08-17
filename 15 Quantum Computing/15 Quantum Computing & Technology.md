#technology #quantum #cybersecurity

Quantum computing harnesses the principles of quantum mechanics to process information in fundamentally different ways than classical computers. While classical computers have made extraordinary progress, certain problems remain computationally intractable — quantum computing promises to crack some of them wide open.

---

## Classical vs Quantum Computing

| Property | Classical Computing | Quantum Computing |
|---|---|---|
| Basic unit | Bit (0 or 1) | Qubit (0, 1, or both simultaneously) |
| Processing | Sequential/parallel logic gates | Quantum gates operating on superpositions |
| State space | N bits = 2^N possible states (one at a time) | N qubits = 2^N states simultaneously |
| Error model | Deterministic, predictable | Probabilistic, noisy |
| Best use cases | General-purpose computation | Factoring, simulation, optimization |

Classical computers are not going away — quantum computers are not universally faster. They excel at specific problem classes.

---

## Core Quantum Mechanics Principles

### Superposition
A qubit can exist in a combination of |0⟩ and |1⟩ simultaneously until measured. Mathematically: |ψ⟩ = α|0⟩ + β|1⟩, where α and β are complex amplitudes and |α|² + |β|² = 1.

- This allows a quantum computer with N qubits to represent 2^N states at once
- Measurement collapses the qubit to a definite 0 or 1
- The art of quantum computing is steering the computation before measurement

### Entanglement
Two or more qubits can become correlated in ways that have no classical analogue. Measuring one instantly determines the state of the other, regardless of physical distance.

- Entanglement is a resource — algorithms use it to coordinate qubits
- Not faster-than-light communication (no information is transmitted)
- Enables quantum teleportation of qubit states and quantum key distribution

### Interference
Quantum states have phases (like waves) that can constructively or destructively interfere.

- Quantum algorithms are designed so correct answer paths interfere constructively (amplified)
- Wrong answer paths interfere destructively (cancelled out)
- This is how quantum algorithms achieve speedup — not by trying all answers at once, but by steering probability toward correct ones

---

## Qubits: Physical Implementations

Different physical systems can act as qubits. Each has different trade-offs in coherence time, gate fidelity, connectivity, and scalability.

### Superconducting Qubits
- **Companies:** IBM, Google, Rigetti
- Tiny circuits cooled to ~15 millikelvin (colder than outer space)
- Fast gate operations (~nanoseconds)
- Short coherence times (microseconds to milliseconds)
- Currently the leading approach for scale — IBM's roadmap targets millions of physical qubits

### Trapped Ions
- **Companies:** IonQ, Quantinuum (Honeywell)
- Individual ions (e.g., Ytterbium) held in electromagnetic traps
- Very high gate fidelity and long coherence times
- Slower gate operations than superconducting
- All qubits naturally connected (all-to-all connectivity)

### Photonic Qubits
- **Companies:** PsiQuantum, Xanadu
- Qubits encoded in photons (particles of light)
- Room temperature operation possible
- Natural fit for quantum networking
- Deterministic two-qubit gates are challenging

### Topological Qubits
- **Companies:** Microsoft (Azure Quantum)
- Based on exotic quasiparticles called Majorana fermions
- Theoretically far more error-resistant (errors are topologically suppressed)
- Still largely pre-experimental — Microsoft announced first topological qubit in 2023
- Could leapfrog other approaches if it matures

---

## Quantum Gates and Circuits

Quantum gates are the quantum analogue of classical logic gates. They are unitary matrices that transform qubit states.

### Single-Qubit Gates
- **Hadamard (H):** Creates superposition from |0⟩ → (|0⟩ + |1⟩)/√2. The workhorse of quantum computing.
- **Pauli-X:** Quantum NOT gate — flips |0⟩ ↔ |1⟩
- **Pauli-Y:** Combines X and Z with a phase
- **Pauli-Z:** Phase flip — leaves |0⟩ unchanged, flips phase of |1⟩
- **Phase gates (S, T):** Rotate the phase of the |1⟩ component

### Two-Qubit Gates
- **CNOT (Controlled-NOT):** Flips the target qubit if the control qubit is |1⟩. Creates entanglement when control is in superposition.
- **CZ (Controlled-Z):** Applies Z to target if control is |1⟩

### Three-Qubit Gates
- **Toffoli (CCNOT):** AND gate for qubits — flips target if both controls are |1⟩. Universal for classical reversible computing.

### Quantum Circuits
- Sequences of gates applied to qubits, read left to right
- Circuits end with measurement operations
- Circuit depth (number of sequential gates) affects noise accumulation

---

## Quantum Error Correction

Qubits are extraordinarily fragile. Interactions with the environment cause **decoherence** (loss of quantum state) and **gate errors**. Unlike classical bits, you cannot simply copy a qubit (no-cloning theorem).

### Error Sources
- **Decoherence:** Environmental noise randomises qubit state
- **Gate errors:** Imperfect operations introduce small errors
- **Readout errors:** Measurement itself is imperfect
- **Crosstalk:** Nearby qubits unintentionally affect each other

### Error Correction Codes
- **Surface codes:** The leading practical approach. Logical qubits are encoded in a 2D lattice of physical qubits. ~1000 physical qubits per logical qubit at current error rates.
- **Shor code:** First quantum error correcting code — encodes 1 logical qubit in 9 physical qubits, corrects arbitrary single-qubit errors
- **Steane code:** 7-qubit code, corrects any single-qubit error

The ratio of physical-to-logical qubits is the key challenge. At current ~0.1–1% physical error rates, thousands of physical qubits are needed per logical qubit. Fault-tolerant quantum computing requires this overhead to be manageable at scale.

---

## Key Quantum Algorithms

### Shor's Algorithm (1994)
- **Problem:** Integer factorisation
- **Speedup:** Exponential — factors an N-bit number in polynomial time vs exponential classically
- **Implication:** Breaks RSA, Diffie-Hellman, and elliptic curve cryptography
- Requires fault-tolerant quantum computer (millions of logical qubits) — not yet achievable
- Motivates post-quantum cryptography urgency now

### Grover's Algorithm (1996)
- **Problem:** Unstructured search (finding an item in an unsorted database)
- **Speedup:** Quadratic — O(√N) vs classical O(N)
- **Implication:** Halves the effective key length of symmetric encryption (AES-128 → ~64-bit effective security)
- Fix: double symmetric key lengths (AES-256 remains safe)

### QAOA (Quantum Approximate Optimisation Algorithm)
- Hybrid classical-quantum algorithm for combinatorial optimisation problems
- Useful for routing, scheduling, portfolio optimisation
- Works on near-term (NISQ) hardware

### VQE (Variational Quantum Eigensolver)
- Hybrid algorithm for quantum chemistry simulation
- Finds ground state energies of molecules
- Applications: drug discovery, materials science, catalyst design
- Near-term applicable — one of the most promising NISQ use cases

### HHL Algorithm
- Solves systems of linear equations exponentially faster than classical
- Useful for machine learning and fluid dynamics
- Requires fault-tolerant hardware with good input/output

---

## Quantum Advantage

Quantum advantage (or quantum supremacy) refers to problems where a quantum computer outperforms the best classical computer.

### Problem Classes Well-Suited for Quantum
- **Factoring and discrete logarithm:** Shor's algorithm
- **Quantum simulation:** Simulating quantum systems (chemistry, materials, drug design) — exponentially hard classically
- **Optimisation:** Combinatorial problems (scheduling, logistics) via QAOA
- **Machine learning:** Potential quantum speedups for certain ML tasks (QML — still debated)
- **Sampling:** Certain probability distributions (basis of Google's supremacy claim)

### Google's 2019 Sycamore Claim
- Google's Sycamore chip (54 qubits) performed a specific sampling task in 200 seconds
- Claimed it would take Summit supercomputer ~10,000 years
- IBM disputed the claim — revised classical algorithms brought it down to days
- Highlights that "quantum supremacy" claims require careful scrutiny of the classical benchmark

---

## Current State: The NISQ Era

**NISQ = Noisy Intermediate-Scale Quantum** — coined by John Preskill (2018)

### Characteristics of NISQ Devices
- 50–1000+ physical qubits
- No error correction (or very limited)
- Limited coherence times and gate fidelity
- Circuit depth constrained by noise
- Not yet capable of running Shor's algorithm at meaningful scale

### NISQ Applications Being Explored
- Quantum chemistry (VQE)
- Combinatorial optimisation (QAOA)
- Quantum machine learning
- Quantum simulation of physics models

### The Path to Fault-Tolerant Quantum Computing
- Requires logical qubits with error rates below ~10^-10
- Needs thousands of physical qubits per logical qubit (with current error rates)
- Progress on better physical error rates could reduce this overhead dramatically
- Microsoft's topological qubit approach targets inherently lower error rates

---

## Hardware Milestones

| Year | Milestone |
|---|---|
| 2019 | Google Sycamore — 53 qubits, quantum supremacy claim |
| 2021 | IBM Eagle — 127 qubits |
| 2022 | IBM Osprey — 433 qubits |
| 2023 | IBM Condor — 1,121 qubits; IBM Heron (error-corrected) |
| 2023 | Microsoft announces first topological qubit |
| 2024 | Google Willow — 105 qubits, significant error correction progress |
| 2025+ | IBM roadmap: fault-tolerant systems targeting 100,000+ physical qubits |

Note: raw qubit count is not the only metric — gate fidelity, connectivity, and coherence time matter enormously.

---

## Quantum Cloud Platforms

You can run quantum programs today without owning hardware:

- **IBM Quantum:** Free access to real quantum hardware via IBM Quantum Network. Uses Qiskit (Python SDK). quantum.ibm.com
- **Google Quantum AI:** Cirq framework, research-focused, limited public access
- **Azure Quantum:** Microsoft's platform — access to IonQ, Quantinuum, and Microsoft hardware. Uses Q# language.
- **AWS Braket:** Amazon's service — access to IonQ, Rigetti, and simulators. Python SDK.

---

## Quantum Networking

### Quantum Key Distribution (QKD)
- Uses quantum mechanics to distribute cryptographic keys with theoretically unbreakable security
- Any eavesdropping disturbs the quantum states and is detectable
- **BB84 protocol (1984):** First QKD protocol — uses photon polarisation states
- **E91 protocol:** Uses entangled photon pairs
- Current limitations: distance (~100km over fibre without repeaters), cost, compatibility with classical infrastructure

### Quantum Repeaters
- Classical signals can be amplified; quantum states cannot be copied (no-cloning)
- Quantum repeaters use entanglement swapping to extend QKD range
- Still largely experimental — key enabler of the quantum internet

### The Quantum Internet Vision
- A network where quantum states can be transmitted between nodes
- Enables: perfectly secure QKD at global scale, distributed quantum computing, quantum sensor networks
- China leads in satellite-based QKD (Micius satellite experiments)
- US Quantum Internet Blueprint (DOE, 2020) outlines a phased roadmap

---

## Cybersecurity Implications

### The Threat to Current Cryptography
- RSA, ECC, and Diffie-Hellman are broken by Shor's algorithm on a sufficiently powerful quantum computer
- Symmetric encryption (AES-256) survives Grover's attack with doubled key length
- Hash functions (SHA-256, SHA-3) remain largely safe

### "Harvest Now, Decrypt Later"
- Nation-state actors may be collecting encrypted traffic today to decrypt once quantum computers are available
- Classified data with long-term sensitivity is already at risk
- The urgency to migrate is now — not when quantum computers arrive

### Post-Quantum Cryptography (PQC)
NIST completed its PQC standardisation process (2022–2024):

- **CRYSTALS-Kyber (ML-KEM):** Key encapsulation mechanism — replaces RSA/ECC for key exchange
- **CRYSTALS-Dilithium (ML-DSA):** Digital signatures
- **FALCON:** Compact digital signatures (lattice-based)
- **SPHINCS+ (SLH-DSA):** Hash-based signatures — conservative choice, well-understood security

All are based on mathematical problems believed hard for both classical and quantum computers (lattice problems, hash functions).

### Migration Considerations
- Hybrid schemes: combine classical and PQC algorithms during transition
- TLS 1.3 + Kyber hybrid already deployed by Cloudflare and others
- Long-lived certificates and infrastructure need priority migration
- See [[Cryptography#Cryptography|Cryptography]] for implementation details

---

## Timeline and Outlook

Forecasts vary widely — quantum computing is genuinely hard to predict:

| Milestone | Estimated Timeline |
|---|---|
| NISQ demonstrations of practical value | Now–2027 |
| Early fault-tolerant devices (100s of logical qubits) | 2028–2032 |
| Cryptographically relevant quantum computer | 2030–2040 (ORCSA estimate) |
| Full fault-tolerant systems | 2035–2050 |

Key uncertainties:
- Physical qubit error rates — any major improvement reshuffles timelines
- Topological qubit viability — could dramatically reduce overhead
- Classical algorithm improvements — sometimes quantum speedups shrink as classical algorithms improve

The safest assumption for security practitioners: treat the threat as real and begin PQC migration now.

---

## Learning Resources

- **IBM Quantum Learning:** learning.quantum.ibm.com — free courses, labs on real hardware
- **Qiskit Textbook:** qiskit.org/learn — comprehensive open-source quantum computing textbook
- **Nielsen & Chuang — "Quantum Computation and Quantum Information":** The definitive academic reference. Dense but complete.
- **Quantum Country:** quantumcountry.com — spaced repetition-based introduction to quantum computing
- **Preskill's Lecture Notes:** theory.caltech.edu/~preskill/ph229/ — graduate-level, free online

---

## Related Notes
- [[Cryptography#Cryptography|Cryptography]]
- [[Space & Satellite Networks|Interplanetary Internet]]
- [[Network Security#Network Security|Network Security]]
- [[Space & Satellite Networks|Space Technology]]
