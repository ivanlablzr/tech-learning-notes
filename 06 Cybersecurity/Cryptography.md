---
type: concept
tags: [cybersecurity, cryptography]
domains: [cybersecurity, mathematics]
maturity: growing
confidence: high
updated: 2026-08-05
---

> [!abstract] Cryptography — the mathematics of securing information: **confidentiality, integrity, authentication, non-repudiation**. This note owns the *concepts* (how systems are built, keyed, and broken); algorithm specifics (AES, RSA, hashing, TLS, signatures) live in [[Data Encryption]].

## What it is

Cryptology = **cryptography** (building secure systems) + **cryptanalysis** (breaking them). The pipeline: **plaintext** → encryption (with a **key** + **algorithm**) → **ciphertext** → decryption → plaintext. The security must rest on the *secrecy of the key*, never the secrecy of the algorithm ([Kerckhoffs's principle](https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle) — assume the enemy knows your system).

## Why it exists / the problem it solves

Communication and storage happen over channels an adversary can read or tamper with (the internet, a stolen disk). Cryptography lets two parties who may never have met achieve four guarantees over a hostile medium: *only the intended reader sees it* (confidentiality), *it wasn't altered* (integrity), *it's really from who it claims* (authentication), and *the sender can't later deny it* (non-repudiation). Every secure protocol you use — TLS, SSH, Signal — is these four properties assembled from primitives.

## How it works — the three cryptosystem types

| Type | Key model | Strength | Weakness |
|---|---|---|---|
| **Symmetric** (AES, ChaCha20) | one shared secret | Fast (<1 ms/MB, HW-accelerated) | **Key distribution** — how do strangers share the secret? |
| **Asymmetric** (RSA, ECC) | public/private pair | Solves distribution; enables **signatures/auth** | ~100× slower |
| **Hybrid** (the real-world standard) | asym to exchange + auth, sym for bulk | Best of both | complexity |

**The hybrid pattern** (used everywhere): generate a random key K → encrypt the message with K (fast symmetric) → encrypt K with the recipient's public key → send both. Every major protocol is hybrid: **TLS 1.3** (ECDHE + certificate auth + AES-GCM/ChaCha20, forward secrecy by default), **SSH** (ECDH + ChaCha20/AES), **Signal** (X3DH + **Double Ratchet** → forward secrecy *and* post-compromise recovery; powers WhatsApp/iMessage). Detail: [[Data Encryption]].

## Keys — the real weak point

A system is only as strong as its keys. Good keys need **unpredictability** (true randomness), **length**, a real **entropy source**, and secure storage.

- **Entropy:** TRNGs from physical noise (RDRAND, TPMs), OS CSPRNGs (`/dev/urandom` / `getrandom()` — safe), or **HSMs** (tamper-resistant; keys never leave the boundary — CAs, payments).
- **KDFs** stretch a password into a key (never use a password directly): PBKDF2 (GPU-parallelisable — weak), bcrypt, scrypt (memory-hard), **Argon2id** (current default — memory + time hard).
- **IVs/nonces** make repeated plaintexts encrypt differently — **unique per encryption, never reused with the same key** (reuse in CTR/GCM catastrophically breaks it). Not secret.

| Algorithm | Min (2025) | Note |
|---|---|---|
| AES (symmetric) | 128-bit | 256 for post-quantum margin (Grover halves it) |
| RSA/DH | 3072-bit | RSA-1024 dead; 2048 legacy-only |
| ECC | 256-bit | Ed25519 preferred (fast, side-channel resistant) |

**Lifecycle:** generation → distribution → storage → rotation → revocation → destruction.

## How crypto breaks (cryptanalysis)

> Usually it's **not the math** — it's the implementation or the human. Modern primitives rarely fall to brute force; systems fall to how they're *used*.

- **Brute-force / mathematical:** feasible only with short keys or broken algorithms (letter-frequency analysis kills classical substitution — 'E' ≈ 12.7% of English).
- **Side-channel (the big real-world threat):** leak the key via **timing**, **power** (DPA on smart cards), EM, or acoustic emissions. Defence: **constant-time code**, masking, shielding.
- **Attack models by access:** ciphertext-only → known-plaintext → chosen-plaintext → chosen-ciphertext (each stronger). Plus **MITM** (defeated by authentication/PKI) and backdoors.
- **Social engineering** sidesteps crypto entirely — the key in someone's head or notes.

## Trade-offs & assumptions

- **Speed vs key-distribution** is the core tension the hybrid model resolves — you *always* pay complexity to get both.
- **Everything assumes good randomness and secret keys.** Break either and the strongest algorithm is worthless. Most real breaches attack these, not the cipher.
- **"Secure" is time-bound.** Key sizes safe today weaken with compute and math advances; **post-quantum** (below) is this assumption expiring for RSA/ECC.
- **Rolling your own is the classic fatal mistake** — use vetted libraries (libsodium, Tink), never hand-built crypto.

## Common misconceptions & mistakes

- **Encoding ≠ encryption.** Base64/ASCII just re-represent bytes — no key, no security (`base64 -d` reverses it instantly).
- **Hashing ≠ encryption** — hashing is one-way (integrity/passwords), not reversible.
- **"AES is unbreakable"** — the algorithm maybe; your IV reuse, weak RNG, or hard-coded key isn't.
- **Fatal implementation bugs:** weak RNG (`rand()`/time-seed), IV/nonce reuse, password-as-key, low KDF iterations, hard-coded keys, key reuse across purposes, no rotation.

## Alternatives / when the concept shifts

- **No shared secret possible, need auth?** → asymmetric + PKI (certificates, web of trust).
- **Just need integrity, not secrecy?** → a hash/MAC, not encryption.
- **Facing quantum adversaries?** → **post-quantum cryptography** (lattice/hash-based: NIST **ML-KEM/Kyber**, **ML-DSA**) — Shor's algorithm breaks RSA/ECC, so migration is underway now for long-lived secrets ("harvest now, decrypt later").

## Open questions

- Practical post-quantum migration at scale — hybrid classical+PQC, and what breaks. ([[15 Quantum Computing & Technology]])
- Can homomorphic encryption (compute on ciphertext) ever become fast enough for general use?
- The long game of side-channel vs constant-time — a defence that never fully finishes.

## Connections
- [[Data Encryption]] — the *algorithm* layer this note defers to (AES, RSA, TLS mechanics)
- [[00 Mathematics]] — number theory (primes, discrete log, lattices) is the hardness crypto rests on
- [[15 Quantum Computing & Technology]] — the threat that expires RSA/ECC and forces PQC
- [[Network Security]] — where crypto becomes TLS/VPN/IPsec on the wire
- [[Information Security & Access]] — crypto delivers the C-I-A guarantees defined there
- [[Digital Forensics & Anti-Forensics]] — encryption as an anti-forensic barrier; keys pulled from memory

## Related
[[06 Cybersecurity]] · [[Cybersecurity Skills Roadmap]] · [[Master Index — Technology Vault]]
