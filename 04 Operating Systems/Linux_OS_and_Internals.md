---
type: note
title: Linux OS & Internals
tags: [linux, operating-systems, os-internals, kernel, cybersecurity, networking, systems]
---


> [!abstract] Purpose
> This note explains Linux from the perspective of **how the operating system actually works internally**. It is designed as a foundation for networking engineering and cybersecurity, especially system administration, penetration testing, red teaming, incident response, and security research.
>
> The goal is not to memorize commands. The goal is to understand the relationship between:
>
> **Hardware → Kernel → System Calls → Processes → Files → Memory → Networking → Security Controls → Applications**

---

> [!info] Implementation reference (hybrid model)
> This is the **Linux-specific implementation** layer. The OS-agnostic *concepts* live in atomic notes — [[Processes]], [[System Calls]], [[Virtual Memory]], [[File Descriptors]], [[Filesystems]], [[Trust Boundaries & Privilege]], [[Sockets]] — which link here for the concrete detail. Narrative chain: [[OS Internals: Processes, I-O, Sockets & Networking]]. Model: [[Master Index — Technology Vault]].

# 1. Linux: The Big Picture

Linux is best understood as an operating-system ecosystem built around the **Linux kernel**.

Strictly speaking:

- **Linux** = the kernel.
- A **Linux distribution** = Linux kernel + user-space software + package manager + libraries + initialization system + configuration + applications.

Examples of distributions:

- Debian
- Ubuntu
- Fedora
- Arch Linux
- RHEL
- Rocky Linux
- Alpine

A simplified architecture:

```text
USER
 │
 ▼
Applications
 │
 ├── Firefox
 ├── SSH client
 ├── Python
 ├── Web server
 └── Database
 │
 ▼
Libraries / Runtime
 │
 ├── glibc
 ├── OpenSSL
 └── Language runtimes
 │
 ▼
System Call Interface
 │
 ▼
LINUX KERNEL
 │
 ├── Process Scheduler
 ├── Memory Manager
 ├── Virtual File System
 ├── Network Stack
 ├── IPC
 ├── Security
 ├── Device Drivers
 └── Kernel Modules
 │
 ▼
HARDWARE
 │
 ├── CPU
 ├── RAM
 ├── Storage
 ├── NIC
 └── Devices
```

The central idea is:

> **Applications operate in user space and request privileged services from the kernel through system calls.**

---

# 2. Linux Kernel Architecture

The Linux kernel is often described as a **monolithic kernel with a modular design**.

This means many core operating-system services execute in kernel space, while functionality can also be extended with dynamically loadable modules.

Major kernel responsibilities include:

```text
Linux Kernel
│
├── Process Management
├── CPU Scheduling
├── Memory Management
├── Virtual Memory
├── Filesystems
├── VFS
├── Networking
├── IPC
├── Device Drivers
├── Security
└── Hardware Management
```

## 2.1 User Space vs Kernel Space

```text
USER SPACE
────────────────────────────
Applications
Shells
Servers
Libraries
Scripts
────────────────────────────
          │
     System Calls
          │
          ▼
KERNEL SPACE
────────────────────────────
Linux Kernel
Drivers
Network Stack
Filesystem
Scheduler
Memory Manager
────────────────────────────
          │
          ▼
HARDWARE
```

User-space programs are isolated from direct privileged hardware access.

The kernel provides controlled interfaces.

Examples:

```text
open()
read()
write()
close()

fork()
execve()

socket()
bind()
listen()
accept()
connect()
```

The exact path through libraries and kernel internals varies, but the conceptual boundary is:

```text
User Program
    │
    ▼
System Call
    │
    ▼
Kernel
    │
    ▼
Resource
```

---

# 3. Linux System Calls

A system call is a controlled transition from user space into kernel space.

Example:

```text
Python / C / Go / Rust program
          │
          ▼
Library API
          │
          ▼
System Call
          │
          ▼
Linux Kernel
          │
          ▼
Resource
```

For example, a program may call an API that ultimately invokes:

```text
open()
read()
write()
socket()
connect()
```

The kernel validates permissions and performs the requested operation.

From a security perspective, system calls are important because they reveal what a process is actually asking the kernel to do.

Security monitoring often cares about activity such as:

```text
Process
   │
   ├── open files
   ├── create processes
   ├── create sockets
   ├── change permissions
   ├── modify memory
   └── access sensitive resources
```

---

# 4. Boot Process

A simplified Linux boot sequence is:

```text
Power On
   │
   ▼
Firmware
BIOS / UEFI
   │
   ▼
Bootloader
GRUB / other bootloader
   │
   ▼
Linux Kernel
   │
   ▼
initramfs
   │
   ▼
Root Filesystem
   │
   ▼
PID 1
systemd / init
   │
   ▼
Services
   │
   ▼
User Login
```

## 4.1 Firmware

Firmware initializes hardware and selects a boot device.

Modern systems commonly use UEFI.

## 4.2 Bootloader

The bootloader loads the kernel and often an initial RAM filesystem.

GRUB is common on Linux systems.

## 4.3 Kernel

The kernel initializes:

- CPU management;
- memory;
- drivers;
- hardware;
- networking;
- filesystem support.

## 4.4 initramfs

The initial RAM filesystem provides temporary user-space functionality required to prepare the real root filesystem.

It may contain:

- storage drivers;
- filesystem modules;
- encryption tooling;
- scripts.

## 4.5 PID 1

The first user-space process is normally PID 1.

On many modern distributions:

```text
PID 1 → systemd
```

PID 1 is special because it becomes the ancestor of processes that no longer have another parent and participates in system initialization and service management.

---

# 5. Processes

A process is a running execution context managed by the kernel.

A process has:

- PID;
- parent PID;
- credentials;
- virtual memory;
- file descriptors;
- environment;
- signal state;
- scheduling information;
- security context.

Example:

```text
PID 1
systemd
│
├── sshd
│   └── bash
│       └── python
│
├── nginx
│   ├── worker
│   └── worker
│
└── journald
```

The process tree is useful in cybersecurity because unexpected parent-child relationships can indicate malicious activity.

For example:

```text
nginx
  └── shell
       └── suspicious process
```

may deserve investigation.

---

# 6. Process Creation

A simplified Unix process model involves:

```text
fork()
   │
   ▼
Child Process
   │
   ▼
execve()
   │
   ▼
New Program
```

The shell can therefore:

```text
Shell
 │
 ├── fork()
 │
 ▼
Child
 │
 └── execve("/usr/bin/ls")
          │
          ▼
         ls
```

The parent shell may then wait:

```text
wait()
```

This model explains why process trees are so important.

---

# 7. Threads

A process provides an address space and resources.

Threads are execution units within that process.

Conceptually:

```text
Process
│
├── Address Space
├── Open Files
├── Sockets
│
├── Thread 1
├── Thread 2
└── Thread 3
```

Threads in the same process generally share:

- memory;
- file descriptors;
- many process resources.

But each thread has its own:

- execution state;
- registers;
- stack.

Multithreading allows a server to handle multiple tasks concurrently.

---

# 8. Process States

A Linux process can exist in different states, such as:

```text
Running / Runnable
Sleeping
Stopped
Zombie
```

A simplified model:

```text
Runnable
   │
   ▼
Running
   │
   ├── waits for I/O → Sleeping
   │
   ├── stopped      → Stopped
   │
   └── exits       → Zombie
```

A zombie process has finished execution but still has an entry containing exit information until its parent collects that status.

---

# 9. Memory Management

Linux provides each process with a **virtual address space**.

Conceptually:

```text
Process Virtual Address Space

High Addresses
┌─────────────────────┐
│ Kernel mapping*     │
├─────────────────────┤
│ Stack               │
│        ↓            │
│                     │
│ Memory mappings     │
│ Shared libraries    │
│                     │
│        ↑            │
│ Heap                │
├─────────────────────┤
│ BSS                 │
│ Data                │
│ Text / Code         │
└─────────────────────┘
Low Addresses
```

The exact layout varies by architecture and security features.

The process sees virtual addresses.

The kernel and CPU memory-management hardware translate them to physical memory.

---

# 10. Virtual Memory

Virtual memory provides:

- process isolation;
- address-space abstraction;
- memory protection;
- efficient memory allocation;
- paging.

Conceptually:

```text
Process
Virtual Address
      │
      ▼
Page Tables
      │
      ▼
Physical Address
      │
      ▼
RAM
```

Memory is divided into pages.

The CPU's memory-management unit uses page tables to translate virtual addresses.

This provides isolation:

```text
Process A
Virtual Memory
     X
     │
     │ blocked
     ▼
Process B Memory
```

Normally, one process cannot simply access another process's memory.

---

# 11. Stack, Heap, Data, Text

A typical process contains regions such as:

### Text

Executable machine code.

### Data

Initialized global/static variables.

### BSS

Zero-initialized global/static data.

### Heap

Dynamic memory allocation.

### Stack

Function calls, local variables, and execution state.

Conceptually:

```text
High Address
    │
    ▼
Stack
    │
    ▼
...
    ▲
    │
Heap
    │
Data
    │
Text
    │
    ▼
Low Address
```

This model is fundamental to understanding:

- memory corruption;
- buffer overflows;
- stack-based exploitation;
- heap exploitation;
- code injection;
- memory forensics.

---

# 12. Filesystem Architecture

Linux follows a unified filesystem hierarchy.

The root is:

```text
/
```

Common directories:

```text
/
├── /bin
├── /boot
├── /dev
├── /etc
├── /home
├── /lib
├── /media
├── /mnt
├── /opt
├── /proc
├── /root
├── /run
├── /sbin
├── /sys
├── /tmp
├── /usr
└── /var
```

Important examples:

### `/etc`

System configuration.

### `/var`

Variable data such as logs and application state.

### `/home`

User home directories.

### `/usr`

User-space programs and libraries.

### `/dev`

Device interfaces.

### `/proc`

Virtual filesystem exposing process and kernel information.

### `/sys`

Virtual filesystem exposing kernel/device information.

---

# 13. Everything Is Not Literally a File

Linux is often summarized as:

> "Everything is a file."

A better interpretation is:

> Linux provides a unified file-like interface to many resources.

Examples include:

```text
Regular files
Directories
Devices
Pipes
Sockets
Terminals
```

Many of these can be accessed through file descriptors.

This is why:

```text
stdin
stdout
stderr
file
pipe
socket
```

can all participate in similar I/O abstractions.

---

# 14. Virtual File System (VFS)

The VFS provides a common abstraction layer between applications and filesystem implementations.

Conceptually:

```text
Application
    │
    ▼
System Call
    │
    ▼
VFS
    │
    ├── ext4
    ├── XFS
    ├── Btrfs
    ├── tmpfs
    └── NFS
```

Applications can use common operations such as:

```text
open()
read()
write()
close()
```

without needing to know the exact filesystem implementation.

---

# 15. Inodes and Dentries

At a high level:

- **inode** represents metadata about a filesystem object;
- **dentry** helps represent directory entries and name-to-object relationships.

An inode can contain information such as:

- ownership;
- permissions;
- timestamps;
- file size;
- pointers to data.

The filename is associated with the directory entry rather than being the inode itself.

Conceptually:

```text
Filename
   │
   ▼
Dentry
   │
   ▼
Inode
   │
   ▼
File Data
```

This distinction becomes useful when studying:

- hard links;
- deleted files;
- filesystem forensics;
- file recovery.

---

# 16. Mounting

Linux presents filesystems through a single directory tree.

A filesystem can be mounted at a directory:

```text
/
├── etc
├── home
└── data
       ▲
       │
     Mount
       │
   Filesystem
```

This allows separate storage devices or filesystem types to appear within the same hierarchy.

---

# 17. File Permissions

Traditional Linux permissions are based on:

```text
User
Group
Other
```

Each has:

```text
r = read
w = write
x = execute
```

Example:

```text
-rwxr-x---
```

Conceptually:

```text
Owner   → rwx
Group   → r-x
Other   → ---
```

Permissions determine whether an operation is allowed, subject to additional mechanisms.

---

# 18. UID and GID

Linux processes operate with user and group identities.

Examples:

```text
UID 0 → root
UID 1000 → normal user
```

A process may have:

- real UID;
- effective UID;
- saved IDs;
- supplementary groups.

The kernel uses credentials when making authorization decisions.

A useful security model is:

```text
Process
   │
   ▼
Credentials
   │
   ├── UID
   ├── GID
   └── Groups
   │
   ▼
Authorization Check
   │
   ▼
Allow / Deny
```

---

# 19. Root

`root` traditionally represents UID 0.

Root has broad privileges, but modern Linux security is more granular than simply:

```text
root vs non-root
```

Modern Linux can use:

- capabilities;
- namespaces;
- seccomp;
- SELinux;
- AppArmor;
- filesystem permissions.

---

# 20. SUID and SGID

SUID executables can execute with the file owner's effective identity.

Conceptually:

```text
Normal User
    │
    ▼
SUID Program
    │
    ▼
Effective Privileges
```

This is security-sensitive.

Misconfigured SUID programs have historically been a common privilege-escalation path.

---

# 21. Linux Capabilities

Capabilities split traditionally broad root privileges into more granular units.

Examples include capabilities related to:

- network administration;
- binding privileged ports;
- changing file ownership;
- debugging other processes.

Conceptually:

```text
Process
   │
   ▼
Capabilities
   │
   ├── Capability A
   ├── Capability B
   └── Capability C
```

A process can therefore have a specific privileged ability without possessing every traditional root privilege.

---

# 22. ACLs

Access Control Lists extend traditional Unix permissions.

They allow more granular rules for specific users and groups.

Conceptually:

```text
File
 │
 ├── Owner permissions
 ├── Group permissions
 ├── Other permissions
 └── ACL entries
```

---

# 23. `/proc`

`/proc` is a virtual filesystem exposing kernel and process information.

Examples:

```text
/proc/1
/proc/<PID>
/proc/cpuinfo
/proc/meminfo
/proc/net
```

A process directory can expose information about:

- command line;
- environment;
- memory mappings;
- file descriptors;
- process status.

Conceptually:

```text
/proc/<PID>/
├── cmdline
├── environ
├── fd/
├── maps
├── status
└── exe
```

This is extremely useful for system administration and incident response.

---

# 24. File Descriptors

Each process has a file descriptor table.

```text
Process
│
├── FD 0 → stdin
├── FD 1 → stdout
├── FD 2 → stderr
├── FD 3 → file
├── FD 4 → pipe
└── FD 5 → socket
```

The kernel maintains the underlying objects.

This is the bridge between:

```text
Process
    ↓
File Descriptor
    ↓
Kernel Object
```

---

# 25. IPC

Linux provides multiple inter-process communication mechanisms.

Examples:

```text
Pipes
Named Pipes / FIFOs
Signals
Unix Domain Sockets
Shared Memory
Message Queues
```

Example:

```text
Process A
    │
    │ write()
    ▼
Pipe
    │
    │ read()
    ▼
Process B
```

Unix domain sockets allow local processes to communicate through a socket-like interface without using IP networking.

---

# 26. Signals

Signals provide asynchronous notifications to processes.

Examples:

```text
SIGTERM
SIGKILL
SIGINT
SIGHUP
SIGSTOP
```

Conceptually:

```text
Process A
   │
   │ signal
   ▼
Process B
```

Signals are used for:

- process control;
- termination;
- interruption;
- service management.

Security tools and malware can use signals to control or manipulate processes.

---

# 27. Services and Daemons

A daemon is a background process that provides a service.

Examples:

```text
sshd
nginx
systemd-journald
cron
```

A typical server might look like:

```text
Network
   │
   ▼
Listening Socket
   │
   ▼
Server Process
   │
   ▼
Worker Process / Thread
   │
   ▼
Application Logic
```

---

# 28. systemd

On many Linux distributions, `systemd` is the primary initialization and service-management system.

It manages:

- services;
- sockets;
- mounts;
- timers;
- targets;
- dependencies.

Example conceptual relationship:

```text
systemd
   │
   ├── ssh.service
   ├── nginx.service
   ├── docker.service
   └── cron.service
```

A service can be started, stopped, enabled, or inspected.

From a security perspective, persistence can involve creating or modifying services, timers, or startup mechanisms.

---

# 29. Linux Networking Architecture

A simplified path:

```text
Application
    │
    ▼
Socket API
    │
    ▼
Linux Socket Layer
    │
    ▼
TCP / UDP
    │
    ▼
IP
    │
    ▼
Routing
    │
    ▼
Netfilter
    │
    ▼
Network Driver
    │
    ▼
NIC
```

An application can create:

```text
socket()
```

and use:

```text
bind()
listen()
accept()
connect()
send()
recv()
```

The kernel implements the networking stack.

---

# 30. Linux Networking Tools

Useful tools include:

```bash
ip addr
ip route
ip link
ss
ping
traceroute
dig
tcpdump
```

These provide visibility into:

- interfaces;
- addresses;
- routes;
- sockets;
- DNS;
- packets.

The important principle is:

> Use tools to observe the underlying mechanisms you already understand.

---

# 31. Network Namespaces

Linux network namespaces provide isolated network environments.

Conceptually:

```text
Host
│
├── Network Namespace A
│   ├── Interfaces
│   ├── Routes
│   └── Sockets
│
└── Network Namespace B
    ├── Interfaces
    ├── Routes
    └── Sockets
```

Containers use namespaces as part of their isolation model.

---

# 32. Netfilter and Firewalling

Linux networking includes the Netfilter framework.

Tools such as:

```text
nftables
iptables
```

interact with packet filtering and network policy.

Conceptually:

```text
Packet
   │
   ▼
Network Stack
   │
   ▼
Netfilter Hooks
   │
   ├── Accept
   ├── Drop
   └── Modify
```

Firewalling is only one layer of security.

A process can still be compromised through an allowed port or application.

---

# 33. Containers

Linux containers rely heavily on kernel features.

Important mechanisms include:

```text
Namespaces
cgroups
Capabilities
seccomp
Filesystem isolation
```

Conceptually:

```text
Container
│
├── Namespace Isolation
├── Resource Limits
├── Restricted Capabilities
├── Filesystem View
└── Process View
```

Containers are not simply "small virtual machines."

They share the host kernel.

---

# 34. SELinux and AppArmor

Linux security can be extended beyond traditional discretionary access controls.

### SELinux

Uses a mandatory access-control model based on security labels and policies.

### AppArmor

Uses profile-based restrictions on application behavior.

Conceptually:

```text
Process
   │
   ├── Traditional Permissions
   │
   ├── Capabilities
   │
   └── MAC Policy
          │
          ▼
       Decision
```

A compromised process may therefore be restricted even if traditional Unix permissions would otherwise allow an operation.

---

# 35. Logging

Important Linux logging mechanisms include:

```text
journald
syslog
application logs
audit logs
```

Security monitoring may examine:

- authentication;
- process creation;
- service activity;
- privilege changes;
- network connections;
- filesystem changes.

---

# 36. Linux Internals for Cybersecurity

When investigating a Linux machine, ask:

```text
What processes are running?
        │
        ▼
Who owns them?
        │
        ▼
What privileges do they have?
        │
        ▼
What files are they accessing?
        │
        ▼
What sockets do they own?
        │
        ▼
What network destinations do they contact?
        │
        ▼
What child processes do they create?
        │
        ▼
What persistence mechanisms exist?
```

Useful investigation areas:

```text
Processes
   ↓
Process Trees
   ↓
File Descriptors
   ↓
Sockets
   ↓
Users / Groups
   ↓
Services
   ↓
Logs
   ↓
Filesystem
   ↓
Kernel Security Controls
```

---

# 37. Linux Security Mental Model

A Linux system can be represented as:

```text
USER
 │
 ▼
APPLICATION
 │
 ▼
PROCESS
 │
 ├── Credentials
 ├── Memory
 ├── File Descriptors
 ├── Capabilities
 └── Namespaces
 │
 ▼
SYSTEM CALLS
 │
 ▼
LINUX KERNEL
 │
 ├── Scheduler
 ├── Memory Manager
 ├── VFS
 ├── Network Stack
 ├── Security Framework
 └── Drivers
 │
 ▼
HARDWARE
```

For cybersecurity, always ask:

> **What can this process do, under which identity, with which resources, and through which kernel interfaces?**

---

# 38. Practical Learning Path

Study Linux internals in this order:

1. Processes and process trees
2. File descriptors and I/O
3. System calls
4. Memory and virtual memory
5. Filesystems and permissions
6. Users, groups, and capabilities
7. Services and systemd
8. Networking and sockets
9. `/proc` and `/sys`
10. IPC
11. Namespaces and containers
12. SELinux/AppArmor
13. Linux logging and auditing
14. Linux security and exploitation

---

# 39. Master Mental Model

```text
HARDWARE
   │
   ▼
LINUX KERNEL
   │
   ├── CPU Scheduling
   ├── Memory
   ├── Filesystems
   ├── Networking
   ├── IPC
   ├── Security
   └── Drivers
   │
   ▼
SYSTEM CALLS
   │
   ▼
USER-SPACE PROCESSES
   │
   ├── Shells
   ├── Web Servers
   ├── Databases
   ├── SSH
   └── Applications
   │
   ▼
USER / NETWORK
```

The core Linux security model is therefore:

> **Processes execute in user space, use kernel interfaces through system calls, and operate under identities and security controls enforced by the kernel.**

This understanding is the foundation for analyzing Linux vulnerabilities, privilege escalation, malware, persistence, process injection, container escapes, network services, and endpoint attacks.

## Related Notes

- [[Operating System & Network Communication Fundamentals]]
- [[Windows OS & Internals]]
- [[Linux Networking]]
- [[Linux Security]]
- [[Processes & File Descriptors]]
- [[Sockets & Network Communication]]
- [[Cybersecurity Fundamentals]]
