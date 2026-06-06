---
type: note
tags: [cybersecurity, cryptography]
---


The mathematics of securing information — providing **confidentiality, integrity, authentication, and non-repudiation**. (Algorithm specifics — AES, RSA, hashing, TLS, signatures — are in [[Data Encryption]]; this note covers the concepts, key management, and how systems are broken.)

## 1. Cryptology fundamentals

**Cryptology** = **cryptography** (building secure systems) + **cryptanalysis** (breaking them). Core terms: **plaintext** → (encryption with a **key** + **algorithm**) → **ciphertext** → (decryption) → plaintext.

> **Encoding ≠ encryption.** Encoding (Base64/Base32, ASCII) just re-represents binary data in a printable character set — *no key, no security* (Base64 maps 6-bit groups to 64 characters; trivially reversible with `base64 -d`). Encryption requires a secret key.

## 2. The three cryptosystem types

- **Symmetric** (one shared secret key — AES, ChaCha20): fast (hardware-accelerated, <1 ms/MB), but the **key-distribution problem** — how do two strangers share the key?
- **Asymmetric** (a public/private key pair — RSA, ECC): solves key distribution and enables **signatures/authentication**, but slow (~100× slower than symmetric).
- **Hybrid** (the universal real-world standard): use **asymmetric to exchange a key + authenticate**, then **symmetric for bulk data**. Pattern: generate random key K → encrypt message with K (fast) → encrypt K with the recipient's public key → send both.

Every major protocol is hybrid: **TLS 1.3** (ECDHE key exchange + certificate auth + AES-GCM/ChaCha20, with **forward secrecy** by default — RSA key exchange removed), **PGP/GPG** (session key + RSA/Ed25519), **SSH** (ECDH + ChaCha20/AES), and **Signal** (X3DH + **Double Ratchet** giving forward secrecy *and* post-compromise recovery — powers WhatsApp/iMessage). Detail in [[Data Encryption]].

## 3. Keys — the real weak point

A system is only as strong as its keys. Good keys need **unpredictability** (true randomness), **sufficient length**, a real **entropy source**, and secure storage.

- **Entropy:** TRNGs from physical noise (Intel RDRAND, TPMs), OS sources (`/dev/urandom` / `getrandom()` — safe; a **CSPRNG** seeded from the entropy pool), or **HSMs** (tamper-resistant; keys never leave the boundary — used by CAs, payments).

| Algorithm | Min (2025) | Notes |
|---|---|---|
| **AES** (symmetric) | 128-bit | AES-256 for long-term / post-quantum margin (Grover halves it to ~128) |
| **RSA/DH** | 3072-bit | RSA-1024 dead; 2048 minimum-legacy |
| **ECC** | 256-bit | P-256 ≈ RSA-3072; **Ed25519** preferred (fast, side-channel resistant) |

- **KDFs** stretch a low-entropy password into a key (never use a password directly): **PBKDF2** (iterated HMAC — GPU-parallelizable), **bcrypt** (cost factor), **scrypt** (memory-hard), and **Argon2id** (the current recommended default — memory + time hard).
- **IVs/nonces** make repeated plaintexts encrypt differently — must be **unique per encryption** and **never reused with the same key** (reuse in CTR/GCM catastrophically breaks confidentiality and forgeability). They're not secret.
- **Common fatal mistakes:** weak RNG (`rand()`/time seed), IV reuse, short keys (RSA-1024/DES), password-as-key, low KDF iterations, hard-coded keys, key reuse across purposes, no rotation.
- **Key management lifecycle:** generation → distribution → storage → rotation → revocation → destruction. The decryption key must stay secret; only the public key is shareable.

## 4. Cryptanalysis — how crypto breaks

Usually it's not the math — it's the implementation or the human.

- **Mathematical/brute-force:** exploit an algorithm weakness, or try all keys (feasible only with short keys; **letter-frequency analysis** breaks classical substitution ciphers — 'E' ≈ 12.7% in English).
- **Side-channel (implementation) attacks** — the big real-world threat: leak the key via **timing**, **power** (SPA/DPA on smart cards), **electromagnetic**, or **acoustic** emissions during operation. Defenses: **constant-time algorithms**, masking/noise, shielding, randomization.
- **Attack models** (by attacker access): ciphertext-only, **known-plaintext**, **chosen-plaintext**, chosen-ciphertext. Plus **MITM** (intercept exchange — defeated by authentication/PKI) and **backdoors**.
- **Social engineering** sidesteps crypto entirely (the key in someone's head/notes).

> **Emerging:** **post-quantum cryptography** (lattice/hash-based — NIST's ML-KEM/Kyber, ML-DSA, since Shor's algorithm breaks RSA/ECC) and blockchain (hashes + signatures). Standards from NIST, ISO, IETF.

Related: [[Data Encryption]] · [[06 Cybersecurity|domain overview]] · [[00 Mathematics|number theory]] · [[Quantum Computing & Technology|quantum threat]] · [[Network Security]]
