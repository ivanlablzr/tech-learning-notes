---
type: concept
tags: [cybersecurity, identity, authentication, ntlm, windows, activedirectory, security]
domains: [cybersecurity, networking, os]
maturity: growing
confidence: high
updated: 2026-08-05
---

> [!abstract] **NTLM** is Microsoft's legacy **challenge-response authentication** protocol — the fallback [[Active Directory]] uses when [[Kerberos]] can't (IP-based access, workgroups, legacy apps). It proves knowledge of a password **hash** without sending the password, but its design (no salting, hash-equals-identity) makes it the source of two of the most important attacks in Windows pentesting: **Pass-the-Hash** and **NTLM relay**.

## What it is
A family (NTLMv1/NTLMv2) of challenge-response protocols. The client proves it knows the account's **NT hash** (MD4 of the UTF-16 password) by encrypting a server challenge, without transmitting the hash or password itself.

## Why it exists
It predates [[Kerberos]] in Windows and still fills gaps Kerberos can't: authenticating to a host by **IP** (Kerberos needs an SPN/DNS name), local/workgroup accounts (no KDC), and old software. It exists for compatibility — and that longevity is its security problem.

## How it works — the three messages
```
1. NEGOTIATE   client → server: "I want to authenticate (NTLM)"
2. CHALLENGE   server → client: a random nonce (challenge)
3. AUTHENTICATE client → server: response = f(NT hash, challenge)
                server (or the DC via netlogon) verifies the response
```
The server can't verify locally for domain accounts — it forwards to a **Domain Controller**. Crucially, the **NT hash itself is the credential**: possessing it is equivalent to knowing the password (there's no salt, no per-login secret).

## State — who owns/reads/writes
- The **NT hash** lives in the SAM (local) or NTDS.dit (domain) and, at runtime, in **LSASS memory** — the theft target ([[Credential Playbook]]).
- No salting → the same password always yields the same hash → precomputation and reuse are trivial.

## Direct dependencies
- [[Cryptography]] — **depends-on** · MD4/MD5/HMAC underpin hashing and the challenge response
- [[Windows_OS_and_Internals]] — **depends-on** · SAM/LSASS/netlogon are the substrate
- [[Trust Boundaries & Privilege]] — **prereq** · authentication establishes *who* crosses a boundary

## Direct effects
- [[Active Directory]] — **enables** · the fallback auth path when Kerberos is unavailable
- lateral movement — **security⚠** · a stolen hash authenticates directly (no cracking needed)

## Failure modes
- **Downgrade** — an attacker forces NTLM instead of Kerberos to enable relay/cracking.
- **Legacy NTLMv1** — cryptographically weak; responses crackable to the NT hash.

## Security implications
- **security⚠ Pass-the-Hash (PtH)** — because the **hash is the credential**, an attacker who dumps a hash from LSASS authenticates as that user **without ever cracking the password**. This is *the* defining Windows lateral-movement technique.
- **security⚠ NTLM relay** — NTLM has no channel binding by default, so an attacker who **coerces** a victim to authenticate to them (LLMNR/NBT-NS poisoning with Responder, or coercion like PetitPotam) can **relay** that authentication to a *third* service (SMB, LDAP, AD CS) and act as the victim. No password or hash cracking required.
- **security⚠ Net-NTLMv2 cracking** — captured challenge/responses (e.g. via Responder) can be cracked offline to recover the password.
- **Defence:** disable NTLM where possible, enable SMB/LDAP **signing** and channel binding, mitigate LLMNR/NBT-NS, and use the Protected Users group. Prefer [[Kerberos]].

## OS implementation (impl ref)
- **Windows:** [[Windows_OS_and_Internals]] §32 (NTLM), §29–31 (authentication, SAM, LSASS). Offensive: [[Credential Playbook]] · [[Technique Catalog]].

## Mechanism graph
```mermaid
flowchart TD
  C[client] -->|NEGOTIATE| S[server]
  S -->|CHALLENGE nonce| C
  C -->|AUTH = f(NT hash, nonce)| S
  S -->|verify via netlogon| DC[Domain Controller]
  HASH[(NT hash in LSASS)] -.security⚠ hash = credential → Pass-the-Hash.- SEC{{lateral movement}}
  C -.coerced auth relayed to.- REL[third service: SMB/LDAP/AD CS]
  REL -.security⚠ NTLM relay.- SEC
```

## Connections
- [[Kerberos]] — **enables** · the stronger protocol NTLM falls back from; downgrade attacks bridge them
- [[Active Directory]] — **enables** · AD's fallback authentication
- [[Cryptography]] — **depends-on** · the hashing it relies on (and its lack of salting)
- [[Credential Playbook]] · [[Technique Catalog]] — **security⚠** · PtH, relay, Responder
- [[Windows_OS_and_Internals]] — **composes** · concrete implementation
- [[Network Security]] — **constrains** · signing/channel binding as defence

## Related
[[Master Index — Technology Vault]] · [[06 Cybersecurity]] · [[Information Security & Access]]
