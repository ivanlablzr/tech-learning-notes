---
type: reference
tags: [meta, resources, news, osint, threat-intel, cybersecurity]
domains: [cybersecurity, networking, ai, geopolitics]
maturity: growing
updated: 2026-07-20
---

> [!abstract] What this note is
> A curated **information-diet** for staying current across tech — weighted toward **cybersecurity & offensive security**, with AI and general tech alongside. Sources to *follow deliberately*, not doomscroll.

> [!tip] Build a system, don't graze
> The point isn't to read everything — it's to build a **pipeline that brings the signal to you** and cut the rest.
> 1. **One RSS reader** (Feedly, Inoreader, or self-hosted **FreshRSS** in your [[16 Home Lab Projects|home lab]]) — most sites below have a feed. This kills the urge to check 20 tabs.
> 2. **2–3 weekly newsletters** for the human-curated digest (see §7). Let editors filter for you.
> 3. **One daily 15-min slot** — pair it with the 🧠 deep-work block in your [[Daily-note-template|daily note]], not with your phone in bed.
> 4. **Follow researchers, not platforms.** One good analyst beats a firehose of headlines.
> Signal-to-noise is the whole game. A weak-area learner ([[Personal weaknesses]]) especially can't afford to confuse *consuming news* with *building skill* — this note supports the skill, it doesn't replace it.

---

## 1. General tech news

| Source | Why | Feed |
|---|---|---|
| [Hacker News](https://news.ycombinator.com) ⭐ | The default front page of serious tech; comments often better than articles | RSS |
| [Ars Technica](https://arstechnica.com) | Deep, technically literate reporting | RSS |
| [The Register](https://www.theregister.com) | Sharp, sceptical enterprise/infosec/British wit | RSS |
| [Lobsters](https://lobste.rs) | Higher signal, more technical than HN | RSS |
| [Hackaday](https://hackaday.com) | Hardware/embedded/maker — feeds your [[02 Electronics|Electronics]] | RSS |

---

## 2. Artificial Intelligence (a bit)

| Source | Why |
|---|---|
| [Import AI](https://importai.substack.com) (Jack Clark) ⭐ | Weekly, policy + research + implications — the best single AI newsletter |
| [The Batch](https://www.deeplearning.ai/the-batch/) (Andrew Ng) | Accessible weekly research roundup |
| [Simon Willison's blog](https://simonwillison.net) ⭐ | The best practical LLM-engineering writing anywhere |
| [Ahead of AI](https://magazine.sebastianraschka.com) (Raschka) | Deeper ML/LLM technical explainers |
| r/LocalLLaMA | Where practical open-model tinkering happens |

> For **learning** AI (not just news), your routes live in [[AI Learning Path & Resources]] and [[IT Certifications and Learning Resources per Domain#Artificial Intelligence]].

---

## 3. Cybersecurity — news & investigative journalism

| Source                                                    | Why                                                                                                                                                                                           | Feed |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- |
| [Krebs on Security](https://krebsonsecurity.com) ⭐        | The gold standard for long-form cybercrime investigation                                                                                                                                      | RSS  |
| [The Record](https://therecord.media) (Recorded Future) ⭐ | Serious threat/nation-state reporting                                                                                                                                                         | RSS  |
| [BleepingComputer](https://www.bleepingcomputer.com)      | Fast breach/ransomware/vuln news + practical fixes                                                                                                                                            | RSS  |
| [The Hacker News](https://thehackernews.com)              | High-volume daily headlines (skim)                                                                                                                                                            | RSS  |
| [SecurityWeek](https://www.securityweek.com)              | Enterprise, ICS/OT, vuln write-ups                                                                                                                                                            | RSS  |
| [Dark Reading](https://www.darkreading.com)               | Analysis & industry trends                                                                                                                                                                    | RSS  |
| [DataBreaches.net](https://databreaches.net)              | Breach-specific tracking                                                                                                                                                                      | RSS  |
| **[Citizen Lab](https://citizenlab.ca)** ⭐                | *Your link.* University of Toronto — the world's leading research on **spyware, surveillance & digital rights** (Pegasus, mercenary spyware). Where journalism meets deep technical forensics | RSS  |
| https://www.haaretz.com/israel-news/security-aviation     |                                                                                                                                                                                               |      |
| https://www.bloomberg.com/technology                      |                                                                                                                                                                                               |      |

---

## 4. Threat intelligence & vulnerability feeds

The operational layer — what's being exploited *right now*. Wire these into your workflow.

| Source | Use |
|---|---|
| [CISA KEV Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) ⭐ | The authoritative "patch these now — actively exploited" list |
| [NVD](https://nvd.nist.gov) / [CVE](https://www.cve.org) | Canonical vulnerability database |
| **[Huntress blog & Threat Library](https://www.huntress.com/blog)** ⭐ | *Your link.* Excellent, readable real-world intrusion analysis (RMM abuse, token replay, SMB threats). Great for learning *how attacks actually run* |
| [Mandiant](https://cloud.google.com/blog/topics/threat-intelligence) | APT & incident deep-dives |
| [Cisco Talos](https://blog.talosintelligence.com) | Broad, high-quality research |
| [Unit 42](https://unit42.paloaltonetworks.com) (Palo Alto) | Malware & campaign analysis |
| [Google Project Zero](https://googleprojectzero.blogspot.com) ⭐ | Elite vuln research — study to learn how bugs are found |
| [abuse.ch](https://abuse.ch) (MalwareBazaar, URLhaus, ThreatFox) | Free live malware/IOC feeds — usable in your lab |
| [GreyNoise](https://www.greynoise.io) · [Shodan](https://www.shodan.io) · [Censys](https://censys.io) | Internet-scan intelligence & attack-surface recon |

---

## 5. Offensive security — learn the craft

| Source | Why |
|---|---|
| [PortSwigger Research](https://portswigger.net/research) ⭐ | The cutting edge of web attacks (feeds BSCP) |
| [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) ⭐ | The offensive cheat-sheet repo — keep it open during [[Ethical Hacking|engagements]] |
| [HackTricks](https://book.hacktricks.xyz) ⭐ | The pentester's field manual; pairs with your [[Technique Catalog]] |
| [GTFOBins](https://gtfobins.github.io) · [LOLBAS](https://lolbas-project.github.io) | Living-off-the-land priv-esc references |
| [Exploit-DB](https://www.exploit-db.com) | Public exploit archive (by OffSec) |
| [0xdf writeups](https://0xdf.gitlab.io) / [IppSec](https://www.youtube.com/ippsec) ⭐ | HTB walkthroughs — the best way to learn methodology |
| **[Xakep (Хакер)](https://xakep.ru)** | *Your link.* Long-running Russian hacker ezine, deep technical content. **Russian-language** — use a translator; a lot of original offensive material surfaces here first. See §11 OPSEC note before registering anywhere |
| [Darknet Diaries](https://darknetdiaries.com) ⭐ | Narrative podcast on real hacks/breaches — the most bingeable in the field |

---

## 6. Podcasts

| Show | Focus |
|---|---|
| [Risky Business](https://risky.biz) ⭐ | The weekly infosec staple — candid, expert, funny |
| [Darknet Diaries](https://darknetdiaries.com) ⭐ | Storytelling on hacks, breaches, cybercrime |
| [CyberWire Daily](https://thecyberwire.com) | Daily concise briefing |
| [Malicious Life](https://malicious.life) | History of hacking & cyber conflict |
| [Talos Takes](https://blog.talosintelligence.com) | 5–10 min threat explainers |
| [Hacking Humans](https://thecyberwire.com/podcasts/hacking-humans) | Social engineering & scams |

---

## 7. Newsletters — let editors filter for you

| Newsletter | Why |
|---|---|
| [tl;dr sec](https://tldrsec.com) (Clint Gibler) ⭐ | The best weekly AppSec/offensive/eng roundup, annotated |
| [Risky Business News](https://news.risky.biz) ⭐ | Concise threat/policy digest |
| [Schneier on Security](https://www.schneier.com) | Crypto, privacy, security-thinking from Bruce Schneier |
| [This Week in Security](https://this.weekinsecurity.com) | Broad weekly |
| [Unsupervised Learning](https://danielmiessler.com/newsletter) (Daniel Miessler) | Security + AI + meaning; good for your cross-domain angle |
| [CTI: Sources & Methods](https://sourcesandmethods.substack.com) | Cyber threat-intelligence sources/tools/tradecraft |

---

## 8. Communities & forums

| Community | For |
|---|---|
| r/netsec ⭐ · r/cybersecurity · r/AskNetsec · r/blueteamsec | Reddit — curated technical vs. career vs. defense |
| r/HowToHack · r/oscp · r/hackthebox | Learning offensive, exam prep |
| [infosec.exchange](https://infosec.exchange) ⭐ | The main **Mastodon** instance — where infosec moved off Twitter; follow researchers here |
| [HTB](https://www.hackthebox.com) & [TryHackMe](https://tryhackme.com) Discords ⭐ | Active help while you do CPTS |
| [NetworkChuck / John Hammond / many] Discords | Community + live help |
| Local **DEF CON group / OWASP chapter / 2600** (Genève, Annecy, Lyon) ⭐ | *In-person matters* — jobs and mentorship come from meeting people. Check DEF CON Groups + OWASP chapter lists for FR/CH |

---

## 9. Who to follow (people > platforms)

Start a single list in your RSS reader or Mastodon: **The Grugq** (OPSEC), **SwiftOnSecurity**, **Marcus Hutchins (MalwareTech)**, **vx-underground** (malware archive), **Katie Moussouris**, **Matt Tait (pwnallthethings)**, **Kevin Beaumont (GossiTheDog)**, **Bruce Schneier**, plus the research orgs above (Citizen Lab, Huntress, Talos, Project Zero). Follow the *analysts*, and the important news reaches you anyway.

---

## 10. 🇫🇷🇨🇭 French & regional (relevant to you)

| Source | Why |
|---|---|
| **[DRSD — guides sûreté entreprises](https://www.defense.gouv.fr/drsd/ressources-entreprises/guides-supports-surete)** ⭐ | *Your link.* French military counter-intelligence (DRSD) — economic-security & counter-espionage guides for firms. Gold for the FR defence/PASSI world you're targeting |
| [ANSSI](https://cyber.gouv.fr) ⭐ | France's cyber agency — guides, [[06 Cybersecurity|hardening]] standards, PASSI framework. Essential for FR employability |
| [CERT-FR](https://www.cert.ssi.gouv.fr) ⭐ | French national CERT — official advisories & alerts (RSS) |
| [MELANI / NCSC Switzerland](https://www.ncsc.admin.ch) | Swiss national cyber centre |
| [LeMagIT](https://www.lemagit.fr) · [Le Monde Informatique](https://www.lemondeinformatique.fr) | French-language tech/security press |
| [Zataz](https://www.zataz.com) (Damien Bancal) | Veteran FR cybercrime/breach journalism |

---

## 11. The deep & dark web — how professionals actually use it

You asked for darknet forums and deep-web links. Here's the honest, useful version — because the reality is different from the myth, and the myth gets people burned.

> [!warning] Read before anything else — legal & safety reality
> - **Merely browsing some dark-web content is a crime** (stolen data, CSAM, some marketplaces) regardless of intent, and much of it is **law-enforcement-run honeypots or scams**. Access ≠ safe.
> - **Never** touch this from a personal or **work** device/network. Given you handle client pentest material ([[Decorec]]), cross-contamination is a real professional and legal risk.
> - Do research-only from an **isolated VM** — **Tails** (amnesic) or **Whonix** — over Tor, no personal accounts, no logins, nothing downloaded/executed.
> - **I'm deliberately not listing criminal marketplace or carding/hacking-forum onion URLs.** They're volatile (change weekly), frequently scams or stings, and providing working access to criminal infrastructure isn't something I'll do. That's not how pros get value anyway — see below.

**How experts *actually* gather dark-web intel (the professional path):**

Analysts and journalists rarely hand-browse criminal forums. They use **CTI platforms** that do the collection, translation and de-risking for them:

| Category | Tools |
|---|---|
| **CTI platforms** (monitor forums/markets for you) | Recorded Future, Flashpoint, Intel 471, KELA, Cybersixgill, DarkOwl — enterprise, but the standard |
| **Search / OSINT over onion & breaches** | [Ahmia](https://ahmia.fi) (clearnet search for legal onion sites), [Intelligence X](https://intelx.io), [Have I Been Pwned](https://haveibeenpwned.com), DeHashed |
| **Leak/ransomware tracking (clearnet)** | [ransomwatch](https://ransomwatch.telemetry.ltd), [ransomware.live](https://www.ransomware.live) — track leak-site activity without visiting the sites |
| **Malware/IOC (clearnet)** | vx-underground, abuse.ch (§4) |

**Legitimate deep-web (.onion) resources — safe & useful:**

These are legal services with onion mirrors, used by journalists, researchers and privacy-conscious pros:

- **[SecureDrop](https://securedrop.org)** — how whistleblowers reach journalists (most major outlets run an instance)
- **[ProPublica](http://p53lf57qovyuvwsc6xnrppyply3vtqm7l6pcobkmyqsiofyeznfu5uqd.onion)**, **BBC**, **NYT** — onion mirrors for censorship-resistant news
- **DuckDuckGo**, **Proton Mail**, **Debian** — onion services of legit tools
- **The Tor Project** itself — start at [torproject.org](https://www.torproject.org) to understand the model

**Known criminal-forum *names* (for awareness — CTI analysts track these; do not treat as an invitation):** the Russian-speaking scene around **XSS.is** and **Exploit.in**, and the recurring **"BreachForums"** lineage, are the ones you'll see referenced constantly in threat reporting. Knowing they exist and what trades there is CTI literacy; you learn about them **through** the reporting in §3–4 and CTI platforms above, not by joining them. **Xakep (§5)** is the legal, clearnet way into a lot of the same Russian-language technical knowledge.

> [!note] The honest takeaway
> "Dark web research" for a defender or pentester is 90% **reading CTI analysis of** these places and 10% carefully-scoped observation from a hardened VM — never casual browsing. If a future role needs real darknet CTI, you'll do it inside an org with legal cover, an OPSEC policy, and a platform subscription. Build the clearnet foundation first.

---

## Connections

- [[06 Cybersecurity]] · [[Ethical Hacking]] · [[Security Operations & IR]] — where this feeds
- [[Cybersecurity Skills Roadmap]] · [[IT Certifications and Learning Resources per Domain]] — *learning* vs *staying-current* (this note is the latter)
- [[Technique Catalog]] · [[Credential Playbook]] — the offensive references above map to these
- [[Internet Geopolitics]] · [[Tech Sovereignty & Governance]] — Citizen Lab / DRSD tie into surveillance & state-cyber
- [[16 Home Lab Projects]] — self-host FreshRSS; test IOCs from abuse.ch safely
- [[Learning Efficiency]] — apply spacing/retrieval so this stays learning, not scrolling

## Related

[[Master Index — Technology Vault]]
