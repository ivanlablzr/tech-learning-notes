---
type: note
tags: [cybersecurity, redteam, forensics, careers, legal]
---


The supporting material around the methodology: the toolkit you work from, the forensics you must understand to stay hidden (and to catch others), how to build a career in this, and the laws that make it legal or criminal.

## 1. Tooling

The platform is **Kali** or **ParrotOS**. The core kit, by job:

| Job | Tools |
|---|---|
| Scanning | **nmap**, masscan |
| Web | **Burp Suite**, `ffuf`/`gobuster` (fuzzing) |
| Exploitation | **Metasploit** |
| Active Directory | **BloodHound**, **impacket**, **Rubeus**, **Mimikatz** |
| Lateral movement | **CrackMapExec** / NetExec |
| Password cracking | **hashcat**, John the Ripper |
| Command & control | **Cobalt Strike**, **Sliver** |
| Network capture | **Responder** (LLMNR/NTLM poisoning) |
| Wi-Fi | aircrack-ng, hcxtools |
| Reverse engineering | **Ghidra**, IDA |

## 2. Digital forensics & anti-forensics

The flip side a red team must understand, because it's what gives you away.

**Forensics** reconstructs what happened from the evidence a system leaves behind: disk artifacts (the MFT, prefetch, event logs, registry hives), **memory** (analyzed with Volatility), and network captures (PCAP) — all handled under a strict chain of custody with image hashes to prove nothing was altered. (See [[Security Operations & IR#3. Digital forensics & evidence|IR forensics]].)

**Anti-forensics** is what adversaries do to defeat that — and what defenders therefore study: clearing or tampering with logs, **timestomping** (faking file timestamps), secure deletion, running **only in memory** so nothing touches disk, and encryption. This is exactly *why* defenders ship logs **off-host** in real time and capture memory before anything else: so the attacker can't erase the evidence that's still only in RAM.

## 3. A career path (purple team)

**Purple teaming** fuses offense and defense — you learn how attacks work *so that* you can detect them, and the insight flows both ways. It's in high demand and well paid.

A realistic 24–36 month arc:

1. **Foundation** — Linux, networking, programming, web basics.
2. **Parallel red + blue** — **OSCP** for offense; **CySA+** plus hands-on **SIEM labs** where you write detections for the very attacks you've learned to run.
3. **Deepen red** — Hack The Box Pro Labs, **CRTO** / Cobalt Strike, building C2 infrastructure.
4. **Deepen blue** — threat hunting, detection engineering, DFIR.

The real differentiator is a **home lab**: run an attack, then build the SIEM detection that catches it. Practice platforms: **Hack The Box**, **TryHackMe**, and CTFs. (Broader career/cert context: [[Cybersecurity Foundations#5. Careers & certifications|careers & certs]].)

## 4. Laws & regulations

None of the above is legal without authorization. Every country regulates computer use, unauthorized access, interception, and data protection differently — know the jurisdiction you operate in. A reference map of the major statutes:

| Category | USA | Europe | UK | India | China |
|---|---|---|---|---|---|
| Protecting critical infrastructure & personal data | [CISA](https://www.cisa.gov/resources-tools/resources/cybersecurity-information-sharing-act-2015-procedures-and-guidance) | [GDPR](https://gdpr-info.eu/) | [Data Protection Act 2018](https://www.legislation.gov.uk/ukpga/2018/12/contents/enacted) | [IT Act 2000](https://www.indiacode.nic.in/bitstream/123456789/13116/1/it_act_2000_updated.pdf) | [Cyber Security Law](https://digichina.stanford.edu/work/translation-cybersecurity-law-of-the-peoples-republic-of-china-effective-june-1-2017/) |
| Criminalizing unauthorized access | [CFAA](https://www.justice.gov/jm/jm-9-48000-computer-fraud) | [NISD 2](https://www.enisa.europa.eu/topics/state-of-cybersecurity-in-the-eu/cybersecurity-policies/nis-directive-2) | [Computer Misuse Act 1990](https://www.legislation.gov.uk/ukpga/1990/18/contents) | [IT Act 2000](https://www.indiacode.nic.in/bitstream/123456789/13116/1/it_act_2000_updated.pdf) | [National Security Law](https://www.chinalawtranslate.com/en/2015nsl/) |
| Anti-circumvention / copyright | [DMCA](https://www.congress.gov/bill/105th-congress/house-bill/2281) | [CoE Cybercrime Convention](https://www.europarl.europa.eu/cmsdata/179163/20090225ATT50418EN.pdf) | — | — | [Anti-Terrorism Law](https://web.archive.org/web/20240201044856/http://ni.china-embassy.gov.cn/esp/sgxw/202402/t20240201_11237595.htm) |
| Interception of communications | [ECPA](https://www.congress.gov/bill/99th-congress/house-bill/4952) | [E-Privacy Directive](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX%3A32002L0058) | [Human Rights Act 1998](https://www.legislation.gov.uk/ukpga/1998/42/contents) | [Indian Evidence Act 1872](https://web.archive.org/web/20230223081850/https://legislative.gov.in/sites/default/files/A1872-01.pdf) | — |
| Health information | [HIPAA](https://aspe.hhs.gov/reports/health-insurance-portability-accountability-act-1996) | — | [Police and Justice Act 2006](https://www.legislation.gov.uk/ukpga/2006/48/contents) | [Indian Penal Code 1860](https://web.archive.org/web/20230324123747/https://legislative.gov.in/sites/default/files/A1860-45.pdf) | — |
| Children's data | [COPPA](https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa) | — | [Investigatory Powers Act 2016](https://www.legislation.gov.uk/ukpga/2016/25/contents/enacted) | — | — |
| Cross-border cooperation / data transfer | — | — | [RIPA 2000](https://www.legislation.gov.uk/ukpga/2000/23/contents) | [Digital Personal Data Protection Act](https://www.meity.gov.in/static/uploads/2025/11/53450e6e5dc0bfa85ebd78686cadad39.pdf) | [Cross-border Data Transfer Measures](https://www.mayerbrown.com/en/perspectives-events/publications/2022/07/china-s-security-assessments-for-cross-border-data-transfers-effective-september-2022) |

Related: [[Ethical Hacking|offensive overview]] · [[Engagement & Methodology]] · [[Security Operations & IR#3. Digital forensics & evidence|IR forensics]] · [[Cybersecurity Foundations#5. Careers & certifications|careers & certs]] · [[Threats & Malware#6. APTs & MITRE ATT&CK|MITRE ATT&CK]]
