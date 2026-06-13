---
type: note
tags: [cybersecurity, redteam, pentesting, offensive]
---


The same methodology applies to every target, but the *techniques* change with the attack surface. This is the catalog — what you attack, and how, on each kind of system. For the inbound section-anchor links from other notes, the aliases below (mobile, hardware, radio, physical, etc.) all point here.

## Web applications

The **OWASP Top 10** in practice — injection (`sqlmap`), cross-site scripting (XSS), **SSRF** into cloud metadata endpoints, **IDOR/BOLA** (accessing objects you shouldn't), authentication/JWT flaws, and file-upload webshells. The standard toolkit is **Burp Suite**. Full breakdown in [[Internet & Application Security#OWASP Top 10|OWASP Top 10]].

## Operating systems & privilege escalation

Once you have a shell, enumerate the host automatically with **LinPEAS/WinPEAS**, then abuse whatever's misconfigured: on Linux, `sudo` rules, SUID binaries, weak service configs, and writable scheduled tasks; on Windows, services, scheduled tasks, and registry autoruns. The goal is to climb from a normal user to `root`/`SYSTEM`.

## Active Directory

The enterprise crown jewel, where most internal engagements are won. **BloodHound** maps attack paths through the domain; the core techniques are **Kerberoasting** and **AS-REP roasting** (cracking service/user account hashes), **Pass-the-Hash** and **Pass-the-Ticket**, **DCSync** (impersonating a domain controller to pull password data), forged **Golden/Silver tickets**, and **ADCS (ESC1)** certificate abuse. Each technique, its detection, and its counter is laid out in [[Credential Playbook]].

## Mobile applications

Two halves. **Static** analysis decompiles the APK/IPA looking for hardcoded secrets and insecure logic; **dynamic** analysis hooks the running app with **Frida**, intercepts its traffic, and inspects insecure local storage. The reference standard is the **OWASP MASVS**.

## Hardware & electronics

Attacking the physical device: **UART/JTAG** debug ports for shell/debug access, dumping firmware off **SPI flash**, **glitching / fault injection** to skip security checks, and sniffing data buses with a **Bus Pirate** or logic analyzer.

## Radio & RF

Software-defined radio (**HackRF**, **RTL-SDR**) to capture, replay, jam, or analyze wireless signals — car key fobs, **RFID/NFC** (with a **Proxmark**), Wi-Fi (handshake capture and cracking), and Bluetooth Low Energy.

## Physical access control

Defeating the building: cloning badges, bypassing locks, tailgating through doors, and beating RFID entry systems. This overlaps heavily with social engineering and on-site red-team operations — see [[OpSec & Physical]].

Related: [[Ethical Hacking|offensive overview]] · [[Engagement & Methodology]] · [[Credential Playbook]] · [[Internet & Application Security#OWASP Top 10|OWASP Top 10]] · [[OpSec & Physical]]
