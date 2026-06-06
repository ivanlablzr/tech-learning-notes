---
type: note
tags: [cybersecurity, encryption, cryptography]
---


The algorithm reference for protecting data **at rest** and **in transit** (the concepts/key-management are in [[Cryptography]]).

## 1. Symmetric encryption

One shared key, fast (hardware **AES-NI**). The dominant cipher is **AES** (128/256-bit), used in a **mode of operation**:

| Mode | Property |
|---|---|
| ECB | ❌ never use — identical blocks → identical ciphertext (leaks patterns) |
| CBC | chains blocks with a random **IV**; needs padding |
| CTR | turns a block cipher into a stream; parallelizable |
| **GCM** | CTR + built-in authentication (**AEAD**) — the modern default |
| **XTS** | tweaked by sector — for disk encryption |

**ChaCha20-Poly1305** is a fast AEAD stream cipher (great without AES hardware — mobile). *IVs/nonces must never repeat under one key.*

## 2. Asymmetric encryption & key exchange

A public/private key pair. **RSA** (factoring hard; 3072-bit+) and **ECC** (discrete-log on curves; P-256/**Ed25519** — same security as RSA-3072 at a fraction of the size). Used for key exchange, signatures, and encrypting small secrets (never bulk data — too slow).

**Key exchange** lets two strangers derive a shared secret over a public channel: **Diffie-Hellman** and **ECDH**; the **ephemeral** variants (DHE/**ECDHE**) give **forward secrecy** (past sessions stay safe if a long-term key later leaks). Post-quantum: **ML-KEM (Kyber)**.

## 3. Hash functions

A one-way function mapping any input to a fixed-size **digest** — **no key**. Properties: deterministic, fast, pre-image resistant (can't reverse), collision resistant (hard to find two inputs with the same hash).

- **MD5, SHA-1 — broken** (collisions); use **SHA-256/384/512 (SHA-2)** or **SHA-3**.
- **Uses:** integrity (compare `sha256sum file.iso` to the published hash), digital signatures (sign the hash), malware identification (VirusTotal), and **password storage** (with a slow KDF — **Argon2id/bcrypt** + salt, never raw SHA).
- **HMAC** = keyed hash for message authentication (used in TLS, JWTs).

## 4. Digital signatures & PKI

**Sign with the private key, verify with the public key** — proving **authenticity + integrity + non-repudiation** (you hash the message, then encrypt the hash with your private key). Algorithms: RSA, ECDSA, **Ed25519**.

**PKI** binds public keys to identities via **X.509 certificates** issued by a **Certificate Authority**. Trust chains: root CA → intermediate → leaf; your browser trusts a few hundred roots. Revocation via **CRL** (lists) or **OCSP** (live queries / stapling). This is what stops MITM on HTTPS — the cert proves the server is who it claims.

## 5. Data at rest

**Full Disk Encryption (FDE)** encrypts the whole volume below the filesystem (block layer); a stolen disk reveals nothing. Pre-boot authentication unlocks it, ideally with a **TPM** that **seals** the key to measured boot state (PCRs) — so the key only releases if the boot chain is untampered (transparent unlock).

| Tool | OS | Cipher / notes |
|---|---|---|
| **BitLocker** | Windows | AES-XTS; protectors (TPM, TPM+PIN, recovery key); AD/Azure backup |
| **LUKS/dm-crypt** | Linux | AES-XTS, Argon2id KDF, up to 32 keyslots; TPM via `systemd-cryptenroll` |
| **FileVault 2** | macOS | XTS-AES-128, Secure Enclave |
| **VeraCrypt** | cross-platform | cascades, high-iteration PBKDF2, **hidden volumes** (deniability) |

> **Cold-boot attack:** DRAM retains keys for seconds after power-off — mitigate with TPM+PIN, full shutdown (not sleep), and hardware **memory encryption** (AMD SME/Intel TME). Modern FDE has <5% overhead (AES-NI); **hardware FDE (TCG Opal)** in the SSD is near-zero but some implementations were buggy (verify before trusting).

**Other at-rest layers:** **file-level** (EFS on NTFS, GPG for individual files), **database** — **TDE** (transparent, encrypts the whole DB files), column-level, and Always Encrypted (data opaque even to DBAs); **cloud storage** — server-side **SSE** (provider-managed), **envelope encryption** (data key wrapped by a KMS key), and **BYOK/HYOK** (bring/hold your own keys). **Homomorphic encryption** (PHE/SHE/**FHE** — BGV/CKKS/TFHE) allows computation on ciphertext without decrypting — powerful for privacy, still slow.

## 6. Data in transit

**TLS** secures most internet traffic (HTTPS, mail, etc.) — a **hybrid** handshake: **ECDHE** key exchange → certificate authentication → **AES-GCM/ChaCha20** bulk encryption, keys via **HKDF**. **TLS 1.3** is the standard (1-RTT handshake, forward secrecy mandatory, weak ciphers removed); TLS 1.0/1.1 and SSL are deprecated/broken.

Other in-transit protection, by layer:
- **Network (L3):** **IPsec** (AH/ESP, IKEv2) — site-to-site VPNs and secure WAN; also DNSSEC, RPKI.
- **Data-link (L2):** MACsec (802.1AE).
- **Application/E2E:** **end-to-end encryption** (Signal Protocol, PGP, S/MIME) where only the endpoints hold keys — even the server can't read it.
- **VPNs:** WireGuard/OpenVPN (L4 tunnels), IPsec (L3) — encrypt traffic across untrusted networks (see [[Network Types & Topologies#4. VPNs|VPNs]]).

Related: [[Cryptography]] · [[06 Cybersecurity|domain overview]] · [[Network Security]] · [[OSI Layers & Protocols#Domain Name System (DNS)|TLS/DNS]] · [[09 Cloud|cloud KMS]]
