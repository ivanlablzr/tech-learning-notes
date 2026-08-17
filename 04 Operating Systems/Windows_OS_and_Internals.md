---
type: note
title: Windows OS & Internals
tags:
  - windows
  - operating-systems
  - os-internals
  - kernel
  - cybersecurity
  - networking
  - active-directory
  - systems
---


> [!abstract] Purpose
> This note explains Windows from the perspective of **how the operating system actually works internally**. It is designed as a foundation for networking engineering and cybersecurity, especially Windows administration, Active Directory, penetration testing, red teaming, incident response, malware analysis, and security research.
>
> The goal is to understand the relationship between:
>
> **Hardware → Windows Kernel → System Calls → Processes → Threads → Objects → Handles → Files → Registry → Networking → Security Model → Applications**

---

> [!info] Implementation reference (hybrid model)
> This is the **Windows-specific implementation** layer. The OS-agnostic *concepts* live in atomic notes — [[Processes]], [[System Calls]], [[Virtual Memory]], [[File Descriptors]] (Windows = Handles), [[Filesystems]] (Windows = NTFS), [[Trust Boundaries & Privilege]] (tokens/SIDs/integrity) — which link here for the concrete detail. Narrative chain: [[OS Internals: Processes, I-O, Sockets & Networking]]. Model: [[Master Index — Technology Vault]].

# 1. The Big Picture

Modern Windows is built around the **Windows NT architecture**.

A simplified model:

```text
USER
 │
 ▼
Applications
 │
 ├── Win32 Applications
 ├── .NET Applications
 ├── PowerShell
 ├── Services
 └── Security Software
 │
 ▼
User-Mode APIs
 │
 ├── Win32
 ├── .NET
 ├── COM
 └── Other APIs
 │
 ▼
NTDLL / System Call Boundary
 │
 ▼
WINDOWS KERNEL MODE
 │
 ├── NT Executive
 ├── Kernel
 ├── Object Manager
 ├── Memory Manager
 ├── Process Manager
 ├── I/O Manager
 ├── Security Reference Monitor
 ├── Configuration Manager
 └── Drivers
 │
 ▼
HARDWARE
```

Windows separates execution into:

```text
User Mode
Kernel Mode
```

This separation protects the operating system from ordinary application failures and limits direct access to privileged resources.

---

# 2. Windows Architecture

A simplified architecture:

```text
USER MODE
────────────────────────────────────
Applications
PowerShell
Services
Win32
.NET
Security Tools
────────────────────────────────────
          │
      API / Syscalls
          │
          ▼
KERNEL MODE
────────────────────────────────────
NT Executive
Kernel
Memory Manager
I/O Manager
Object Manager
Security Reference Monitor
Configuration Manager
Device Drivers
────────────────────────────────────
          │
          ▼
HARDWARE
```

---

# 3. User Mode vs Kernel Mode

### User Mode

Most applications run here.

Examples:

```text
explorer.exe
powershell.exe
chrome.exe
notepad.exe
```

Applications have restricted privileges.

### Kernel Mode

Core operating-system components and drivers run here.

Kernel-mode code has significantly greater access to system resources.

Conceptually:

```text
User Mode
    │
    │ System Call / API
    ▼
Kernel Mode
    │
    ▼
Hardware / Kernel Resource
```

A vulnerability in user-mode software may compromise a process.

A vulnerability in a kernel-mode driver can potentially compromise the entire system.

---

# 4. The Windows NT Executive

The Windows NT Executive contains major operating-system subsystems.

Conceptually:

```text
NT Executive
│
├── Object Manager
├── Process Manager
├── Memory Manager
├── I/O Manager
├── Security Reference Monitor
├── Configuration Manager
└── Plug and Play / Power Management
```

These components cooperate to provide operating-system functionality.

---

# 5. The Windows Kernel

The Windows Kernel is responsible for lower-level functionality such as:

- thread scheduling;
- interrupt handling;
- synchronization;
- low-level processor management.

The Windows NT architecture is often visualized as:

```text
Applications
      │
      ▼
Subsystems / APIs
      │
      ▼
NT Executive
      │
      ▼
Windows Kernel
      │
      ▼
Hardware Abstraction Layer
      │
      ▼
Hardware
```

The boundaries are more complex in practice, but this is a useful conceptual model.

---

# 6. Hardware Abstraction Layer

The HAL abstracts certain hardware-specific details from the rest of Windows.

Conceptually:

```text
Windows Kernel
      │
      ▼
HAL
      │
      ▼
Hardware
```

This allows Windows to support different hardware platforms without every kernel component directly handling every hardware-specific detail.

---

# 7. Windows Boot Process

A simplified modern boot process:

```text
Power On
   │
   ▼
UEFI
   │
   ▼
Windows Boot Manager
bootmgr
   │
   ▼
Windows Boot Loader
winload.efi
   │
   ▼
Windows Kernel
ntoskrnl.exe
   │
   ▼
Boot Drivers
   │
   ▼
Session Manager
smss.exe
   │
   ▼
Services / Logon
   │
   ▼
User Environment
```

The exact boot process is more complex, but the major idea is:

```text
Firmware
   ↓
Boot Manager
   ↓
Boot Loader
   ↓
Kernel
   ↓
System Initialization
   ↓
Services
   ↓
User Logon
```

---

# 8. Processes

A process represents a running program and its associated resources.

Conceptually:

```text
Process
│
├── Virtual Address Space
├── Threads
├── Handles
├── Security Token
├── Environment
├── Loaded Modules
└── I/O Resources
```

Examples:

```text
explorer.exe
powershell.exe
svchost.exe
lsass.exe
services.exe
winlogon.exe
```

A process is not the same thing as a program file.

```text
Executable on Disk
       │
       ▼
Process Created
       │
       ▼
Virtual Address Space
       │
       ├── Code
       ├── Data
       ├── Heap
       └── Stack
```

---

# 9. Threads

A process contains one or more threads.

```text
Process
│
├── Thread 1
├── Thread 2
└── Thread 3
```

Threads share much of the process's resources:

- address space;
- handles;
- loaded modules.

Each thread has its own execution state.

This is important for:

- concurrency;
- application performance;
- debugging;
- malware analysis;
- process injection.

---

# 10. Process Trees

Windows processes form parent-child relationships.

Conceptually:

```text
services.exe
   │
   ├── svchost.exe
   │
   ├── service.exe
   │
   └── other process
```

Another example:

```text
explorer.exe
   │
   └── powershell.exe
```

Process trees are extremely important in security monitoring.

Unexpected relationships can indicate malicious behavior.

For example:

```text
web server
   └── powershell.exe
```

may require investigation.

---

# 11. Handles

Windows uses **handles** as references to kernel-managed objects.

A process may have:

```text
Process
│
├── Handle 0x100 → File
├── Handle 0x104 → Process
├── Handle 0x108 → Thread
├── Handle 0x10C → Registry Key
└── Handle 0x110 → Event
```

Conceptually:

```text
User-Mode Process
      │
      │ Handle
      ▼
Kernel Object
```

This is conceptually similar to Unix file descriptors, but Windows handles refer to a much broader object model.

---

# 12. Windows Object Manager

The Object Manager manages kernel objects and their namespaces.

Objects may include:

- processes;
- threads;
- files;
- events;
- mutexes;
- sections;
- tokens;
- registry-related objects;
- other kernel resources.

A process interacts with objects through handles.

Conceptually:

```text
Process
    │
    ▼
Handle Table
    │
    ▼
Kernel Object
```

This abstraction is fundamental to Windows internals.

---

# 13. Processes and Security Tokens

A Windows process has an associated security context.

A major component is the **access token**.

Conceptually:

```text
Process
   │
   ▼
Access Token
   │
   ├── User SID
   ├── Group SIDs
   ├── Privileges
   ├── Integrity Level
   └── Other security information
```

When a process attempts to access a protected resource, Windows evaluates the security context.

---

# 14. Security Identifiers (SIDs)

Windows identifies security principals using SIDs.

Examples include:

- user accounts;
- groups;
- computer accounts.

Conceptually:

```text
User
   │
   ▼
SID
   │
   ▼
Access Token
```

Security decisions use these identities.

---

# 15. Access Control Lists

Windows uses Access Control Entries (ACEs) inside Access Control Lists (ACLs).

Important concepts:

### DACL

Defines who is allowed or denied access.

### SACL

Defines what access attempts are audited.

Conceptually:

```text
Object
   │
   ├── DACL
   │    ├── Allow User A
   │    └── Deny User B
   │
   └── SACL
        └── Audit Access
```

Security decision:

```text
Process Token
      │
      ▼
Object Security Descriptor
      │
      ▼
DACL / Permissions
      │
      ▼
Allow / Deny
```

---

# 16. Integrity Levels

Windows uses Mandatory Integrity Control.

Common integrity levels include:

```text
Low
Medium
High
System
```

A process's integrity level helps restrict interactions with objects at different integrity levels.

This contributes to protections such as User Account Control.

---

# 17. Privileges

Windows privileges are special rights assigned to security tokens.

Examples include privileges related to:

- debugging;
- backup;
- restoring;
- loading drivers;
- impersonation.

A privilege is different from a normal filesystem permission.

Security analysis should therefore consider:

```text
User
   │
   ▼
Token
   │
   ├── SIDs
   ├── Groups
   ├── Privileges
   └── Integrity Level
```

---

# 18. Memory Management

Windows uses virtual memory.

Conceptually:

```text
Process
Virtual Address Space
       │
       ▼
Page Tables
       │
       ▼
Physical Memory
```

A process has regions such as:

```text
Code
Data
Heap
Stack
DLLs
Memory Mappings
```

The OS provides:

- process isolation;
- memory protection;
- paging;
- virtual address spaces.

---

# 19. Windows Virtual Address Space

Conceptually:

```text
Process Virtual Address Space

┌─────────────────────┐
│ User-mode memory    │
│                     │
│ Code                │
│ DLLs                │
│ Heap                │
│ Stack               │
│ Mapped files        │
└─────────────────────┘

Kernel-mode address space
```

Modern Windows systems use architecture-dependent layouts and protections.

Security features can include:

- DEP;
- ASLR;
- CFG;
- memory integrity mechanisms.

---

# 20. Process Environment Block

The Process Environment Block (PEB) is a user-mode data structure associated with a process.

It contains information used by the process environment and loader.

Security researchers and malware analysts often encounter the PEB when studying:

- loaded modules;
- process environment;
- API resolution;
- malware behavior.

The PEB is an implementation detail and should not be treated as a stable application API.

---

# 21. Heaps and Stacks

A process uses:

```text
Heap
```

for dynamic memory allocation.

And:

```text
Stack
```

for thread execution state and function calls.

Conceptually:

```text
Process
│
├── Heap
│
├── Stack (Thread 1)
├── Stack (Thread 2)
└── Stack (Thread 3)
```

Memory corruption vulnerabilities may target:

- stack memory;
- heap memory;
- object metadata;
- function pointers.

---

# 22. Windows I/O Architecture

Windows uses the I/O Manager to coordinate I/O operations.

Conceptually:

```text
Application
    │
    ▼
Win32 API
    │
    ▼
NT Layer
    │
    ▼
I/O Manager
    │
    ▼
Driver Stack
    │
    ▼
Hardware
```

This architecture is used for:

- files;
- network devices;
- disks;
- keyboards;
- USB devices.

Drivers are critical because kernel-mode driver vulnerabilities can have severe consequences.

---

# 23. Windows File Systems

The most important modern Windows filesystem is NTFS.

NTFS provides:

- files;
- directories;
- permissions;
- metadata;
- journaling;
- alternate data streams;
- reparse points.

---

# 24. NTFS and the MFT

The Master File Table (MFT) is a central NTFS structure containing records describing filesystem objects.

Conceptually:

```text
NTFS Volume
    │
    ▼
MFT
    │
    ├── File Record
    ├── File Record
    └── File Record
```

Security and forensic analysis can examine filesystem metadata to understand:

- file creation;
- modification;
- deletion;
- timestamps;
- attributes.

---

# 25. Alternate Data Streams

NTFS supports Alternate Data Streams (ADS).

Conceptually:

```text
File
 ├── Main Data Stream
 └── Alternate Data Stream
```

ADS can be legitimate, but historically they have also been abused to hide data.

Security tools and forensic analysts should therefore understand them.

---

# 26. Windows Registry

The Windows Registry is a hierarchical configuration database.

Conceptually:

```text
Registry
│
├── HKEY_LOCAL_MACHINE
├── HKEY_CURRENT_USER
├── HKEY_CLASSES_ROOT
├── HKEY_USERS
└── HKEY_CURRENT_CONFIG
```

The registry stores information related to:

- operating-system configuration;
- applications;
- users;
- services;
- startup mechanisms;
- hardware.

---

# 27. Registry Hives

Registry data is stored in hive files.

Examples include:

```text
SYSTEM
SOFTWARE
SAM
SECURITY
NTUSER.DAT
```

The Registry is important for:

- system configuration;
- persistence;
- forensic analysis;
- malware investigation.

---

# 28. Windows Services

Windows services are background processes managed by the Service Control Manager.

Conceptually:

```text
Service Control Manager
       │
       ├── Service A
       ├── Service B
       └── Service C
```

A service may start:

- during boot;
- automatically;
- manually;
- in response to triggers.

Security relevance includes:

- persistence;
- privilege boundaries;
- service misconfigurations;
- unquoted service paths;
- weak service permissions.

---

# 29. Windows Authentication

Windows authentication can involve multiple mechanisms.

Important components include:

```text
Local Authentication
    │
    ├── SAM
    └── Local Security Authority

Domain Authentication
    │
    ├── Active Directory
    ├── Kerberos
    └── NTLM
```

---

# 30. SAM

The Security Accounts Manager (SAM) stores information associated with local user accounts.

Conceptually:

```text
Local User
   │
   ▼
SAM
   │
   ▼
Authentication
   │
   ▼
Access Token
```

The SAM is highly sensitive from a security perspective.

---

# 31. LSASS

The Local Security Authority Subsystem Service (LSASS) is responsible for important local security functions.

It participates in:

- authentication;
- security policy;
- credential handling.

Because of its role, it is a major security target.

Defenders monitor unusual access to LSASS.

---

# 32. NTLM

NTLM is a legacy Windows authentication protocol still present in some environments.

It uses challenge-response mechanisms.

It is generally less desirable than Kerberos in modern domain environments.

Security concerns include:

- credential relay;
- password cracking;
- legacy compatibility risks.

---

# 33. Kerberos

Kerberos is the primary authentication protocol used by Active Directory domains.

Conceptually:

```text
Client
   │
   │ Authentication
   ▼
KDC
   │
   ├── Authentication Service
   └── Ticket Granting Service
            │
            ▼
         Service Ticket
            │
            ▼
          Service
```

Kerberos uses tickets rather than repeatedly sending passwords to services.

Important concepts:

- KDC;
- TGT;
- service tickets;
- SPNs.

Kerberos is fundamental to understanding Active Directory security.

---

# 34. Active Directory

Active Directory is Microsoft's directory and identity platform.

Core concepts:

```text
Domain
Domain Controller
Users
Groups
Computers
Organizational Units
Group Policy
Trusts
Kerberos
LDAP
DNS
```

A simplified model:

```text
Client
   │
   ▼
Domain Controller
   │
   ├── Active Directory
   ├── LDAP
   ├── Kerberos
   └── DNS
```

---

# 35. Domain Controllers

A Domain Controller provides domain authentication and directory services.

It commonly hosts:

```text
Active Directory Domain Services
Kerberos
LDAP
DNS
```

Compromise of a domain controller can have severe consequences because it sits at the center of domain identity and authorization.

---

# 36. Group Policy

Group Policy allows administrators to centrally configure systems.

It can manage:

- security settings;
- software;
- scripts;
- configuration;
- user policies.

Conceptually:

```text
Domain
   │
   ▼
Group Policy
   │
   ▼
Computer / User
   │
   ▼
Configuration
```

Group Policy is important both for administration and security analysis.

---

# 37. Windows Networking

A simplified architecture:

```text
Application
    │
    ▼
Winsock / Network APIs
    │
    ▼
Windows Networking Stack
    │
    ▼
TCP / UDP
    │
    ▼
IP
    │
    ▼
Network Driver
    │
    ▼
NIC
```

Windows applications use APIs such as Winsock to communicate over networks.

---

# 38. Windows Filtering Platform

Windows Filtering Platform (WFP) provides infrastructure for network filtering and security enforcement.

It supports components such as:

- Windows Firewall;
- security products;
- network filtering layers.

Conceptually:

```text
Network Traffic
      │
      ▼
WFP
      │
      ├── Allow
      ├── Block
      └── Inspect / Filter
```

---

# 39. SMB

Server Message Block (SMB) is a major Windows network protocol.

It is used for:

- file sharing;
- printer sharing;
- remote administration;
- inter-system communication.

SMB is highly important in Windows security because it is deeply integrated into enterprise environments.

Security topics include:

- authentication;
- permissions;
- signing;
- relay attacks;
- lateral movement.

---

# 40. Windows IPC

Windows provides multiple inter-process communication mechanisms.

Examples:

```text
Named Pipes
RPC
COM
DCOM
Shared Memory
Events
Mutexes
```

These allow processes to communicate locally or remotely.

---

# 41. PowerShell

PowerShell is both:

- a command shell;
- a scripting language;
- an automation framework.

Unlike traditional Unix pipelines, PowerShell pipelines pass structured objects.

Conceptually:

```text
Command A
   │
   │ Objects
   ▼
Command B
   │
   │ Objects
   ▼
Command C
```

PowerShell is deeply integrated with Windows management APIs.

From a security perspective, PowerShell is important for:

- administration;
- automation;
- security operations;
- incident response;
- attack detection.

---

# 42. Windows Command Execution

A simplified model:

```text
User
   │
   ▼
Terminal
   │
   ▼
PowerShell / cmd.exe
   │
   ▼
Process Creation
   │
   ▼
Windows API / Native API
   │
   ▼
Kernel
```

A shell is therefore only one way to initiate operations.

Windows applications can also directly use APIs without an interactive shell.

---

# 43. Event Logging

Windows has extensive logging capabilities.

Important mechanisms include:

```text
Windows Event Log
ETW
PowerShell Logging
Security Auditing
```

Logs can provide information about:

- logons;
- process creation;
- privilege changes;
- service activity;
- authentication;
- PowerShell execution.

---

# 44. ETW

Event Tracing for Windows (ETW) provides a high-performance event tracing framework.

Conceptually:

```text
Application / Kernel
        │
        ▼
ETW Provider
        │
        ▼
ETW Session
        │
        ▼
Consumer
```

Security products can use telemetry from ETW and other sources to detect suspicious behavior.

---

# 45. Windows Defender and AMSI

Windows Defender provides built-in security capabilities.

AMSI (Antimalware Scan Interface) provides an interface through which applications can submit content for security inspection.

Conceptually:

```text
Application
    │
    ▼
AMSI
    │
    ▼
Security Provider
    │
    ▼
Detection / Decision
```

AMSI is particularly relevant to script-based security monitoring.

---

# 46. Endpoint Detection and Response

EDR products monitor endpoint activity.

A simplified model:

```text
Process Creation
       │
       ├── Parent Process
       ├── Child Process
       ├── Command Line
       ├── Network Connections
       ├── File Activity
       ├── Registry Activity
       └── Memory Activity
              │
              ▼
            EDR
              │
              ▼
          Detection
```

This illustrates why modern cybersecurity is increasingly based on **behavior and telemetry**, not only signatures.

---

# 47. Windows Security Mental Model

A Windows process can be modeled as:

```text
PROCESS
 │
 ├── Threads
 ├── Virtual Memory
 ├── Handles
 ├── Modules
 ├── Security Token
 │     ├── User SID
 │     ├── Groups
 │     ├── Privileges
 │     └── Integrity Level
 │
 └── Network Connections
```

When it accesses an object:

```text
Process
   │
   ▼
Access Token
   │
   ▼
Security Descriptor
   │
   ├── DACL
   └── SACL
   │
   ▼
Authorization Decision
```

---

# 48. Windows Internals for Cybersecurity

When investigating a Windows system, ask:

```text
What processes are running?
        │
        ▼
Who launched them?
        │
        ▼
What is the parent-child process tree?
        │
        ▼
What user and token does each process have?
        │
        ▼
What privileges and integrity levels exist?
        │
        ▼
What handles does the process have?
        │
        ▼
What files and registry keys does it access?
        │
        ▼
What network connections does it make?
        │
        ▼
What services and persistence mechanisms exist?
        │
        ▼
What events were logged?
```

---

# 49. Common Windows Security Investigation Areas

```text
Process Trees
     ↓
Command Lines
     ↓
Security Tokens
     ↓
Services
     ↓
Registry
     ↓
Scheduled Tasks
     ↓
PowerShell
     ↓
Network Connections
     ↓
Authentication Logs
     ↓
Active Directory
```

---

# 50. Linux vs Windows: Conceptual Comparison

| Concept | Linux | Windows |
|---|---|---|
| Kernel | Linux kernel | Windows NT kernel |
| User/kernel boundary | User space / kernel space | User mode / kernel mode |
| Process identity | UID/GID | Security token / SID |
| File permissions | rwx, ACLs | ACLs / security descriptors |
| Process I/O handle | File descriptor | Handle |
| Filesystem abstraction | VFS | I/O Manager / filesystem stack |
| Main filesystem | ext4, XFS, Btrfs | NTFS |
| Configuration | Files under `/etc` and others | Registry + files |
| Service manager | systemd commonly | Service Control Manager |
| Shell | Bash, Zsh, etc. | PowerShell, cmd.exe |
| Networking API | POSIX/BSD socket APIs | Winsock |
| Firewall framework | Netfilter/nftables | WFP / Windows Firewall |
| Mandatory controls | SELinux/AppArmor | Integrity levels and other security controls |
| Directory service | Various | Active Directory |
| Primary enterprise auth | Depends on environment | Kerberos / AD |

This table is a conceptual comparison, not a claim that the systems are internally equivalent.

---

# 51. The Biggest Conceptual Difference

A useful high-level contrast is:

### Linux

```text
Process
 │
 ├── UID/GID
 ├── File Descriptors
 ├── Capabilities
 ├── Namespaces
 └── Filesystem Permissions
```

### Windows

```text
Process
 │
 ├── Security Token
 │    ├── SIDs
 │    ├── Groups
 │    ├── Privileges
 │    └── Integrity Level
 │
 ├── Handles
 ├── Object Manager
 └── Security Descriptors
```

Both systems solve similar fundamental problems:

```text
Who are you?
What are you allowed to do?
What resources can you access?
How does the kernel enforce this?
```

They simply implement the mechanisms differently.

---

# 52. Master Windows Mental Model

```text
HARDWARE
   │
   ▼
WINDOWS KERNEL
   │
   ├── Scheduler
   ├── Memory Manager
   ├── I/O Manager
   ├── Object Manager
   ├── Security Reference Monitor
   └── Drivers
   │
   ▼
SYSTEM CALL / API BOUNDARY
   │
   ▼
USER-MODE PROCESSES
   │
   ├── Applications
   ├── Services
   ├── PowerShell
   ├── Security Software
   └── Network Services
   │
   ▼
USER / NETWORK
```

The Windows security model can be summarized as:

> **Processes execute in user mode, interact with kernel-managed objects through handles, and operate under security tokens whose identities, privileges, and integrity levels influence authorization decisions.**

For enterprise cybersecurity, add:

> **Active Directory, Kerberos, LDAP, DNS, Group Policy, SMB, and Windows authentication extend the security model across multiple machines and create a distributed identity and trust system.**

---

# 53. Practical Learning Path

Study Windows internals in this order:

1. Processes and threads
2. Process trees
3. Handles and objects
4. User mode vs kernel mode
5. Windows APIs and system calls
6. Virtual memory
7. NTFS
8. Registry
9. Security tokens and SIDs
10. ACLs and security descriptors
11. Services
12. PowerShell
13. Windows networking
14. Authentication
15. Active Directory
16. Kerberos and NTLM
17. Event Logs and ETW
18. Defender, AMSI, and EDR
19. Windows security architecture
20. Windows exploitation and post-exploitation concepts

---

# 54. Final Takeaway

The most useful Windows mental model is:

```text
Application
    │
    ▼
Process
    │
    ├── Threads
    ├── Virtual Memory
    ├── Handles
    ├── Modules
    └── Security Token
           │
           ├── User SID
           ├── Group SIDs
           ├── Privileges
           └── Integrity Level
    │
    ▼
Windows APIs / Native Interfaces
    │
    ▼
Windows Kernel
    │
    ├── Object Manager
    ├── Process Manager
    ├── Memory Manager
    ├── I/O Manager
    ├── Security Reference Monitor
    └── Drivers
    │
    ▼
Hardware / Network
```

The core questions for cybersecurity are:

> **What process is running?**

> **Who created it?**

> **Under which security token is it executing?**

> **What privileges does it have?**

> **What objects and handles can it access?**

> **What files, registry keys, and network resources does it interact with?**

> **What authentication system established its identity?**

> **What telemetry records its behavior?**

Once these questions become intuitive, Windows security topics such as **privilege escalation, credential theft, process injection, persistence, lateral movement, Active Directory attacks, Kerberos abuse, malware, EDR detection, and incident response** become much easier to understand.

## Related Notes

- [[Operating System & Network Communication Fundamentals]]
- [[Linux OS & Internals]]
- [[Windows Networking]]
- [[Windows Security]]
- [[Active Directory]]
- [[Kerberos]]
- [[Processes, Threads & Handles]]
- [[Sockets & Network Communication]]
- [[Cybersecurity Fundamentals]]
