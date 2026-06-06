---
type: domain
tags: [domain, moc, operating-systems]
---



The **resource manager and trust boundary** that multiplexes one machine across many programs. Many programs, one CPU, limited memory, shared devices — the OS arbitrates, providing scheduling, memory management, device abstraction, isolation, and security.

## Domain notes

| Note | Covers |
|---|---|
| [[Linux]] | Architecture, admin, boot recovery, storage, performance, troubleshooting, hardening |
| [[Windows]] | Architecture, NT internals, registry, services, security model |

## OS concepts (the theory under both)

- **Kernel & system calls.** The **kernel** runs privileged (Ring 0); applications run isolated (Ring 3) and request services via **system calls** — the only controlled door between them. Kernel designs: **monolithic** (Linux), **microkernel** (Minix), **hybrid** (Windows, macOS).
- **Processes & threads.** A process = a running program with its own memory and file descriptors, created by `fork`/`exec` (Unix) or `CreateProcess` (Windows). Threads share a process's memory. The **scheduler** decides what runs next; **context switching** saves/restores CPU state. Memory layout = stack/heap/code/data, with **virtual memory** + paging giving each process an isolated address space.
- **Filesystems.** Organize and protect data: journaling (crash safety), formats (ext4/XFS Linux, NTFS/ReFS Windows, APFS macOS), metadata (inodes, timestamps), links (hard/soft), and permissions (rwx, ACLs). Forensics and persistence live here.
- **Access control & authentication.** Linux: PAM, `/etc/shadow`, sudo/SUID. Windows: SAM, Active Directory, UAC. The basis of privilege boundaries — and the target of privilege-escalation attacks.
- **IPC.** Processes coordinate via pipes, shared memory, sockets, signals, and (Windows) named pipes/RPC.
- **Device drivers** bridge the kernel to hardware; a buggy driver can panic the whole system.

> *Security framing:* hardening the OS ([[System Hardening]], [[Linux#13. Hardening checklist|Linux hardening]]) reduces attack surface; **[[06 Cybersecurity|OS hacking]]** abuses these same mechanisms (process injection, SUID abuse, token theft). Both require the concepts above.

## The five views

**Dependencies:** builds on [[03 Computer Hardware]] (CPU, memory, I/O); depended on by [[05 Networking]] (sockets/stack), [[07 Programming]] (syscalls, threads, files), [[06 Cybersecurity]] (isolation), and [[09 Cloud]]/[[10 DevOps]] (containers are OS features).
```
Hardware → OS → (Networking, Programming, Security, Containers)
```

**Bridge topic it owns — containers:** namespaces (pid/net/mnt/user) + cgroups + union filesystems + images. Containers are not VMs — they are **kernel-isolated processes**, the substrate of [[10 DevOps|Kubernetes]]. It's also the canonical home of **concurrency** (reused by databases and distributed systems) and a link in the [[03 Computer Hardware|boot/trust chain]].

**Learning path:** kernel/user space & syscalls → processes (fork/exec) → threads & concurrency (locks, deadlock) → scheduling → virtual memory/paging → filesystems (VFS, journaling) → I/O & drivers → Linux internals (namespaces/cgroups) → OS security (MAC, capabilities, seccomp).

**Projects:** Linux infrastructure lab (L1 anchor) → a container from scratch (namespaces + cgroups + chroot, no Docker) → a tiny shell (fork/exec/wait, pipes, signals) → an eBPF probe to watch the kernel live. See [[Learning Paths & Projects#Project Roadmap]].

Related: [[03 Computer Hardware]] · [[05 Networking]] · [[06 Cybersecurity]] · [[Knowledge Graph]] · [[Home]]
