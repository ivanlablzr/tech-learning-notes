---
type: note
tags: [cybersecurity, redteam, pentesting, offensive, shells]
---

> ⚠️ **Authorized testing only.** Everything here assumes written permission or your own [[Home Lab]]. Without authorization, gaining a shell on a system is a crime ([[Tooling, Forensics & Careers#Laws & regulations|laws]]). This is the **"Shells & Payloads"** stage of the [[Hacking Engagement & Methodology|methodology]] — turning code execution into control. The *command-interpreter* side of "shell" (bash/zsh/PowerShell).

In offense, **getting "a shell" means obtaining interactive command execution on a compromised host.** It's the payoff of exploitation and the doorway to post-exploitation. Untangle three words:
- **Exploit** — the technique that triggers the flaw (e.g. an upload bug, an RCE).
- **Payload** — what runs *after* the exploit lands (the thing that gives you the shell).
- **Shell** — the interactive session you end up controlling (a bash/`cmd`/PowerShell prompt on the target).

## 1. Bind vs reverse shell — the core distinction (connection *direction*)
Both give you a remote prompt; they differ in **who connects to whom**, which is entirely about **[[Ports, Interfaces & Sockets|ports and firewalls]]**.

```
BIND shell:    Attacker  ───connect──▶  Target (LISTENS on a port)
REVERSE shell: Attacker (LISTENS)  ◀──connect───  Target  (calls back OUT)
```

| | **Bind shell** | **Reverse shell** |
|---|---|---|
| Who listens | the **target** opens a port | the **attacker** opens a port |
| Who initiates | attacker connects **in** | target connects **out** |
| Beaten by | inbound firewall / NAT on target (common) | egress filtering (less common) |
| When to use | target directly reachable, no inbound filtering | **the default** — most networks allow outbound |

The attacker's listener is typically a netcat catch: `nc -lvnp 4444` (listen, verbose, no-DNS, port 4444). Reverse shells win in practice because most firewalls/NAT block inbound but permit outbound (see port **states** in [[Ports, Interfaces & Sockets]] §6). The actual payload one-liners (bash, python, PowerShell, `nc`, socat, etc.) are catalogued in standard references — **revshells.com**, PayloadsAllTheThings, HackTricks, pentestmonkey's cheat-sheet (https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#summary) — pick one matching what's installed on the target.

## 2. Web shell
A **script uploaded to a web server that runs OS commands via HTTP** — used after a file-upload flaw, LFI/RFI, or web RCE. It's written in the server's language: **PHP, ASPX, JSP**. Minimal idea: a page that takes a `cmd` parameter and executes it, so `?cmd=whoami` returns the output. Tools: **weevely**, China Chopper, or the web shells bundled with Kali (`/usr/share/webshells`).
- **Pros:** persists on disk, blends into normal HTTP/443 traffic, survives reboots.
- **Cons:** usually non-interactive and clunky; noisy on disk (file-integrity/AV/WAF can catch it). Often you use a web shell just to launch a proper **reverse shell**. Ties to [[Internet & Application Security|web attacks]] / OWASP.

## 3. Payload types & tooling
- **Staged vs stageless** — *staged* sends a tiny stub that pulls the rest over the network (smaller, needs a second connection); *stageless* is the whole payload at once (bigger, more robust).
- **Interactive vs non-interactive** — a full shell you type into vs one-shot command execution.
- **Meterpreter** (Metasploit) — a feature-rich, in-memory payload (file transfer, pivoting, token theft) that avoids writing a disk artifact. Generated with **msfvenom**, caught by a Metasploit `multi/handler`.
- **Native vs interpreted** — a compiled binary vs a script relying on an interpreter present on the target (python/perl/php/PowerShell).

## 4. Upgrading a "dumb" shell to a full TTY
A raw reverse/bind shell has **no proper [[Shells & Command Line|TTY]]** — no tab-completion, no job control, `Ctrl-C` kills your whole session, no `sudo`/interactive prompts. Stabilise it (on your own foothold):
1. Spawn a PTY: `python3 -c 'import pty; pty.spawn("/bin/bash")'` (or `script -qc /bin/bash /dev/null`).
2. Background with `Ctrl-Z`, then on *your* box `stty raw -echo; fg` to pass keys through raw.
3. Set `export TERM=xterm` and resize (`stty rows … cols …`) so `less`/editors work.
`socat` gives a fully interactive TTY in one step if it's available on both ends. This is pure usability tradecraft — core CPTS skill.

## 5. Where it sits in the kill chain
```
Exploit (get code execution) → PAYLOAD delivers a SHELL → stabilise (TTY upgrade)
   → post-exploit: enumerate, privesc, persist → lateral movement ([[Credential Playbook]])
```
See the full flow in [[Hacking Engagement & Methodology]]; techniques by surface in [[Technique Catalog]].

## 6. Detection & defense (offense informs defense)
Every technique here has a matching defence — the purple-team half:
- **Egress filtering** (default-deny outbound) neutralises most **reverse shells** — the single highest-value control.
- **EDR / AMSI / PowerShell script-block logging** flag malicious PowerShell and in-memory payloads ([[Endpoint Security]]).
- **Web-shell detection** — file-integrity monitoring, WAF, and alerting on web-server processes spawning shells.
- **Network anomalies** — unexpected listeners (`ss -tulpn`), odd outbound connections, beaconing.
- **Least privilege + segmentation** limit what a landed shell can reach ([[System Hardening]], [[Network Security]]).

## Related
[[Shells & Command Line]] · [[Ports, Interfaces & Sockets]] · [[Hacking Engagement & Methodology]] · [[Technique Catalog]] · [[Credential Playbook]] · [[Tooling, Forensics & Careers]] · [[Endpoint Security]] · [[Home Lab]] · [[Ethical Hacking|offensive overview]]
