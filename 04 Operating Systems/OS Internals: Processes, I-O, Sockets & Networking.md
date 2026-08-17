---
type: moc
role: mechanism-chain
tags: [operating-systems, shell, cli, terminal, kernel, processes, file-descriptors, sockets, networking, tcp-ip, cybersecurity]
domains: [os, networking, cybersecurity]
maturity: stable
updated: 2026-08-05
---

> [!abstract] What this note is
> The **narrative map** of how a program reaches the network — the single mental model that makes SSH, reverse/bind/web shells, firewalls, EDR and C2 obvious rather than memorised. It is a **mechanism chain** ([[Master Index — Technology Vault]]): each rung below links to an atomic concept note that holds the depth. *(Split from a 2,400-line monolith — the depth moved into the atomic notes; this note keeps the story and the diagrams.)*

## The one model to internalise

```text
Program → Process → System Calls → File Descriptor → Kernel Object → Socket
        → Kernel Network Stack → TCP/UDP → IP → Ethernet/Wi-Fi → NIC → Network
```

Each arrow is a real mechanism, not just "leads to." Follow the chain rung by rung:

| Rung | Atomic note | One-line mechanism |
|---|---|---|
| **Terminal / Shell / CLI** | [[Shells, Terminals & the CLI]] | I/O interface + command interpreter; a shell is *just a process*, not the kernel |
| **Process** | [[Processes]] | a running program: PID, address space, credentials, fd table; born via `fork`+`execve` |
| **System call** | [[System Calls]] | the privileged boundary crossing (ring 3 → ring 0); *the* trust boundary |
| **File descriptor** | [[File Descriptors]] | a process-local integer handle to a kernel object (file, pipe, socket) |
| **Memory / isolation** | [[Virtual Memory]] | per-process address space (MMU) — why processes can't read each other |
| **Socket** | [[Sockets]] | the kernel communication endpoint an fd points at; the OS↔network bridge |
| **TCP** | [[TCP]] | the state machine turning IP datagrams into a reliable ordered stream |
| **Ports / addressing** | [[Ports, Interfaces & Sockets]] | the (protocol, IP, port) identity and the 5-tuple |

## The complete path (reference diagram)

```text
APPLICATION → PROCESS → SYSTEM CALL → FILE DESCRIPTOR → KERNEL SOCKET
 → SOCKET BUFFER → KERNEL NETWORK STACK → TCP/UDP → IP
 → ROUTING/ARP → NETWORK DRIVER → NIC → ETHERNET/WI-FI → NETWORK
```
Reverse it for receive: `NETWORK → NIC → … → SOCKET → RECEIVE BUFFER → FD → SYSCALL → PROCESS → APPLICATION`.

> The shell did **not** perform `curl https://example.com` — it *launched* `curl`, and the `curl` process did the socket work. Keeping "who does what" straight at each rung is the whole point.

## The distinctions that matter

```text
Terminal ≠ Shell ≠ Kernel
Socket   ≠ Network protocol
TCP      ≠ Application protocol (HTTP/SSH/DNS ride on TCP)
Reverse shell = shell + outbound connection + I/O wired to a socket
Bind shell    = shell + listening socket + inbound connection
Web shell     = web-accessible interface + server-side OS interaction
```
The reverse/bind/web-shell mechanics live in [[Shells, Terminals & the CLI]]; the offensive payloads in [[Shells & Payloads]]. Application vs transport vs network layering: [[OSI Layers & Protocols]].

## The security lens (why this chain is the CPTS foundation)

Don't ask "is there a shell?" Ask of any [[Processes|process]]: *which user owns it, with what privileges, holding which [[File Descriptors|fds]]/[[Sockets|sockets]], listening on which ports, able to reach which networks, able to spawn which children?* A `www-data` web process spawning a child shell is the whole story of a compromise — and it's just this chain, read as an attacker.

## What to study next (per the original note)

Processes/threads/`fork`/`execve`/signals/IPC · Linux I/O (pipes, named pipes, PTY/TTY, unix sockets) · socket API · TCP state machine/seq/retransmit, ARP, routing, network namespaces · privileges, capabilities, egress filtering, EDR, process trees · offensive: initial access, reverse/bind/web shells, tunneling, pivoting, C2.

## Connections
- [[Master Index — Technology Vault]] — **composes** · this note *is* the reference mechanism chain
- [[Shells, Terminals & the CLI]] · [[Processes]] · [[System Calls]] · [[File Descriptors]] · [[Virtual Memory]] · [[Sockets]] · [[TCP]] · [[Ports, Interfaces & Sockets]] — the rungs (depth lives in each)
- [[Shells & Payloads]] — **security⚠** · offensive catalog built on this chain
- [[OSI Layers & Protocols]] — **bridges** · the layered protocol view

## Related Obsidian Notes
[[04 Operating Systems]] · [[Linux]] · [[Windows]] · [[07 Programming]] · [[System Hardening]] · [[Endpoint Security]] · [[16 Home Lab Projects|Home Lab]]
