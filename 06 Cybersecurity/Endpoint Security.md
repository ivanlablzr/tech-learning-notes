---
type: note
tags: [cybersecurity, endpoint, edr, hardware-security]
---


Protecting individual hosts (workstations, servers, laptops, mobiles) — the layer where most attacks land. Spans detection software (AV → EDR), data/device controls, and the **hardware root of trust** everything else depends on.

## 1. Antivirus & anti-malware

Detection has evolved through four approaches, used in combination:

| Method | How it works | Weakness |
|---|---|---|
| **Signature** | matches known-malware hashes/byte patterns | blind to new/polymorphic malware |
| **Heuristic** | flags suspicious code structure/instructions | false positives |
| **Behavioral** | watches runtime actions (mass file encryption, LSASS access) | needs execution |
| **ML / cloud reputation** | classifies by features + global telemetry | adversarial evasion |

Modern AV adds **sandboxing** (detonate suspicious files in isolation) and is increasingly folded into EDR. Signatures still matter for cheap, accurate detection of known threats; behavior/ML catch the rest.

## 2. EDR / XDR / MDR

**EDR (Endpoint Detection & Response)** is the modern endpoint agent: it continuously records telemetry (process trees, file/registry/network activity), detects via behavioral analytics mapped to **MITRE ATT&CK**, and enables **response** — isolate the host, kill processes, roll back changes, and hunt across the fleet. Crucially it catches **living-off-the-land** attacks (PowerShell, WMI, `certutil`) that have no malware signature, via parent-child process analysis (Word→cmd→powershell) and network correlation.

- **XDR** extends correlation across endpoint + network + cloud + identity + email for a unified attack picture.
- **MDR** is EDR/XDR delivered as a managed service (a vendor SOC runs it for you).
- Tools: CrowdStrike Falcon, Microsoft Defender for Endpoint, SentinelOne, Palo Alto Cortex XDR. Feed alerts to a **SIEM/SOAR** ([[Security Operations & IR]]).

## 3. Device control & DLP

Limit what can connect and what can leave:
- **Peripheral control** — restrict/audit USB and removable media (a classic malware-ingress and data-exfil path); allowlist device IDs.
- **Data Loss Prevention (DLP)** — detect and block sensitive data (PII, source, secrets) leaving via USB, email, cloud upload, or print, by content classification.
- **Application control / allowlisting** — only approved binaries run (WDAC/AppLocker, ASR rules).
- **Full-disk encryption** — BitLocker/LUKS/FileVault so a lost device is inert ([[Data Encryption#5. Data at rest|FDE]]).
- **MDM/UEM** — push policy, compliance, and remote wipe to managed devices (Intune, Jamf).

## 4. Hardware security — the root of trust

Compromise below the OS defeats all software controls, so security is anchored in hardware.

**TPM (Trusted Platform Module)** — a crypto microcontroller that generates/stores keys (never exported in plaintext), provides a hardware TRNG, and records boot measurements into **PCRs** (24 hash registers, *extended* not overwritten: `PCR[n] = SHA256(PCR[n] || measurement)`). **Sealing** binds a secret to a platform state — BitLocker seals its key to PCRs (firmware, bootloader, Secure Boot); if any changes, the TPM refuses to unseal. **TPM 2.0** (required by Windows 11) adds SHA-256/ECC over 1.2's SHA-1/RSA-only.

**Measured Boot & attestation** — each boot stage hashes the next before running it and records it in a PCR; the TPM can **sign a quote** of PCR values (with its AIK) so a remote verifier confirms the machine booted to a known-good state.

**Secure Boot (UEFI)** — firmware only loads bootloaders/kernels signed against its key database (**PK** → **KEK** → **db** allowed / **dbx** revoked). Chains trust UEFI → bootloader → kernel; on Linux, custom kernels enroll a **MOK**. (Measured Boot *records* what booted; Secure Boot *blocks* unauthorized boot — complementary.)

**Other hardware roots:** **Secure Enclave / TrustZone** (isolated key handling on phones/Macs), **HSMs** (tamper-resistant key vaults for CAs/payments), and CPU **memory encryption** (AMD SME/SEV, Intel TME) against cold-boot/physical attacks. Hardware threats to defend against: firmware/UEFI implants, malicious peripherals (BadUSB), DMA attacks (mitigated by IOMMU), and supply-chain tampering.

Related: [[06 Cybersecurity|domain overview]] · [[System Hardening]] · [[Data Encryption#5. Data at rest|FDE & TPM]] · [[Network Security#3. Intrusion detection & prevention (IDS/IPS)|HIDS]] · [[Threats & Malware#1. Malware taxonomy|malware]] · [[Security Operations & IR]]
