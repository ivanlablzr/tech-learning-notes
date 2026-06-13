---
type: note
tags: [cybersecurity, activedirectory, credentials, redteam]
---


The path from a single foothold to total domain control in a Windows/Active Directory environment — and, just as importantly, how each step gets caught. Read it from both sides: as an attacker it's a route, as a defender it's a detection checklist.

## The attack chain

| Technique | What it steals / forges | How it's detected | Counter |
|---|---|---|---|
| **Password spray** | valid creds (one password × many users, to dodge lockout) | distributed low-rate auth failures | smart lockout, MFA |
| **LSASS dump** | NTLM hashes of logged-on users | LSASS accessed by a non-system process (Sysmon EID 10) | Credential Guard, RunAsPPL |
| **Pass-the-Hash** | reuses an NTLM hash without cracking it | NTLM auth from workstations to unusual hosts | disable NTLM, tiered admin, Protected Users |
| **Kerberoasting** | a service account's TGS ticket → cracked offline | burst of TGS-REQs, RC4 (0x17) from a workstation | AES keys, **gMSA**, monitor event 4769 |
| **AS-REP roasting** | hash of accounts with pre-auth disabled | AS-REQ without pre-auth (EID 4768) | require Kerberos pre-auth |
| **Pass-the-Ticket** | injects a stolen TGT/TGS | a ticket used from a different host than it was issued to | Credential Guard, short ticket lifetimes |
| **Golden Ticket** | a forged TGT (signed with the krbtgt hash) | anomalous ticket lifetimes, forged usernames | rotate krbtgt **twice**, Defender for Identity |
| **DCSync** | replicates password data like a domain controller | `DsGetNCChanges` (EID 4662) from a non-DC | restrict replication rights |
| **ADCS ESC1** | a certificate for any user → persistent access | cert request with an odd SAN (EID 4886/4887) | fix the template, audit the CA |

Read top to bottom, it's a natural progression: get *a* credential (spray), steal better ones from memory (LSASS), reuse them without cracking (PtH/PtT), crack account hashes (Kerberoasting/AS-REP), then forge your own access (Golden Ticket, DCSync, ADCS) for persistence that survives password resets.

## Detection blind spots

Techniques that are hard to catch because they look like legitimate activity — worth knowing as both attacker and defender:

- **LOLBins** ("living off the land") — signed, built-in OS binaries like `certutil`, `regsvr32` (Squiblydoo), `mshta`, `wmic`, `rundll32`, `bitsadmin`, and `netsh portproxy`. They carry no malware signature, so they're caught only by **parent-child process** anomalies and network behavior, not by AV.
- **Kerberoasting** that blends into normal service discovery.
- **DCSync** that looks like ordinary DC-to-DC replication.
- **Long-dwell APTs** that move slowly enough to blend into the baseline.
- **Domain-fronted C2** over HTTPS (443) to allowlisted CDNs — defeated by **JA3 fingerprinting** and beacon-timing analysis rather than by blocking the destination.

Map every technique to its **MITRE ATT&CK** technique ID so findings line up with the defender's detection coverage (see [[Threats & Malware#6. APTs & MITRE ATT&CK|MITRE ATT&CK]]).

Related: [[Ethical Hacking|offensive overview]] · [[Engagement & Methodology#3.5 Lateral movement — spread|lateral movement]] · [[Technique Catalog#Active Directory|AD techniques]] · [[Security Operations & IR]] · [[Threats & Malware#6. APTs & MITRE ATT&CK|MITRE ATT&CK]]
