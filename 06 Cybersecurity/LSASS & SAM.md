---
type: concept
tags: [cybersecurity, windows, credentials, lsass, sam, activedirectory, security]
domains: [cybersecurity, os]
maturity: growing
confidence: high
updated: 2026-08-05
---

> [!abstract] **SAM** and **LSASS** are where Windows keeps credentials — **SAM** the local account hashes on disk, **LSASS** the live credential material (hashes, [[Kerberos|tickets]], sometimes plaintext) in memory for logged-on users. On a Domain Controller the equivalent is **NTDS.dit**. This is the **theft target** that [[NTLM|Pass-the-Hash]], [[Kerberos|Golden Tickets]], and lateral movement all depend on — the single most valuable prize on a compromised host.

## What it is
- **SAM (Security Account Manager)** — a registry hive (`%SystemRoot%\System32\config\SAM`) storing **local** account NT hashes, readable only as SYSTEM.
- **LSASS (Local Security Authority Subsystem Service)** — the process enforcing the security policy and **caching credentials of interactive sessions** in memory so users don't re-auth constantly.
- **NTDS.dit** — on a [[Active Directory|Domain Controller]], the database holding *every domain account's* hashes (incl. **krbtgt**).

## Why it exists
Windows must verify logons (SAM/NTDS) and provide **single sign-on** — a user authenticates once, then reaches many resources without re-typing a password. To do that, LSASS must *keep* credential material (or derivatives) resident for the session's lifetime. That convenience is precisely the security liability: live credentials sit in memory.

## How it works
- At interactive logon, LSASS validates against SAM (local) or the DC (domain) and **caches** the resulting secrets: NT hashes, [[Kerberos]] TGT/session keys, and — with legacy providers (WDigest) — sometimes cleartext.
- Subsequent auth ([[NTLM]] challenge-response, [[Kerberos]] ticket use) is served from this cached material without the password.
- SAM/NTDS hashes are `SYSTEM`-protected on disk; LSASS memory is `SYSTEM`/PPL-protected at runtime.

## State — who owns/reads/writes
- **SAM/NTDS.dit** — on-disk, SYSTEM-only; extracted offline via VSS or `reg save`.
- **LSASS memory** — runtime creds; reading it requires SYSTEM/debug privilege — the whole game is *getting* that access.

## Direct dependencies
- [[Windows_OS_and_Internals]] — **depends-on** · LSASS, the LSA, and the SAM/registry are Windows subsystems
- [[Trust Boundaries & Privilege]] — **prereq** · reading these requires crossing to SYSTEM
- [[Cryptography]] — **depends-on** · hashes and ticket keys are cryptographic material

## Direct effects
- [[NTLM]] — **enables** · the NT hashes here feed Pass-the-Hash
- [[Kerberos]] — **enables** · cached TGTs here feed Pass-the-Ticket; krbtgt (NTDS) feeds Golden Tickets
- [[Active Directory]] — **security⚠** · NTDS extraction = the whole domain's credentials

## Failure modes
- (Security-dominated — the "failure" is unauthorized extraction; see below.)

## Security implications
- **security⚠ LSASS dumping** — with SYSTEM, dump LSASS memory (`mimikatz sekurlsa::logonpasswords`, `comsvcs.dll` minidump, procdump) → harvest hashes, tickets, sometimes plaintext for every logged-on user. The pivotal post-exploitation step.
- **security⚠ SAM/SYSTEM extraction** — grab the SAM+SYSTEM hives → offline NT hashes of local accounts (local admin reuse enables spraying across hosts).
- **security⚠ DCSync / NTDS.dit** — on/against a DC, replicate or export NTDS → **all** domain hashes incl. krbtgt → [[Kerberos|Golden Ticket]], total domain compromise.
- **Defence:** **Credential Guard** (virtualization-based isolation of LSASS secrets), **LSASS PPL** (protected process), disable WDigest cleartext, LAPS (unique local-admin passwords to kill reuse), EDR watching LSASS handle access, Protected Users group.

## OS implementation (impl ref)
- **Windows:** [[Windows_OS_and_Internals]] §30 (SAM), §31 (LSASS), §29 (authentication), §32–33 (NTLM, Kerberos). Offensive: [[Credential Playbook]] (Mimikatz) · [[Technique Catalog]].

## Mechanism graph
```mermaid
flowchart TD
  LOGON[interactive logon] --> LSASS[LSASS memory: hashes, tickets, (cleartext)]
  SAM[(SAM hive: local NT hashes)] --> LSASS
  NTDS[(NTDS.dit on DC: all domain hashes + krbtgt)]
  LSASS -->|serves| NTLMK[NTLM / Kerberos auth]
  LSASS -.security⚠ dump = harvest creds.- THEFT{{Pass-the-Hash / Pass-the-Ticket}}
  NTDS -.security⚠ DCSync = Golden Ticket.- DOM{{domain compromise}}
```

## Connections
- [[NTLM]] — **enables** · NT hashes → Pass-the-Hash
- [[Kerberos]] — **enables** · cached tickets → PtT; krbtgt → Golden Ticket
- [[Active Directory]] — **security⚠** · NTDS = whole-domain credentials
- [[Credential Playbook]] · [[Technique Catalog]] — **security⚠** · the dumping tradecraft
- [[Trust Boundaries & Privilege]] — **prereq** · SYSTEM access gates it
- [[Windows_OS_and_Internals]] — **composes** · the concrete subsystems
- [[Digital Forensics & Anti-Forensics]] — **security⚠** · LSASS access is a prime detection signal

## Related
[[Master Index — Technology Vault]] · [[06 Cybersecurity]] · [[Endpoint Security]]
