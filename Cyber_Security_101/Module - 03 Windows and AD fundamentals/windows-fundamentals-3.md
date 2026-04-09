# Room 03: Windows Fundamentals 3
# TryHackMe — Cyber Security 101 | Module 03: Windows and AD Fundamentals


## Overview

This room focuses on the **security features built into Windows** — the tools Microsoft provides to protect the OS out of the box. Understanding these is critical for a SOC analyst because these are the first line of defence on every Windows endpoint you'll ever monitor or investigate.

---

## Windows Updates

**Windows Update** delivers security patches, feature updates, and bug fixes for Windows and Microsoft products (including Defender).

**Patch Tuesday** — Updates are typically released on the **second Tuesday of every month**. Critical zero-day patches can be pushed outside this schedule.

**Command to open Windows Update:**
```
control /name Microsoft.WindowsUpdate
```

**Key points:**
- Windows 10 and later cannot permanently skip updates — only postpone them
- Updates eventually force a reboot automatically
- Definition updates (for Defender) are separate from OS updates


## Windows Security

Windows Security is the central dashboard for managing all built-in security tools. Accessible from Settings or the system tray.

**Protection status icons:**

| Icon | Meaning |
|------|---------|
| 🟢 Green | Device is sufficiently protected |
| 🟡 Yellow | Safety recommendation to review |
| 🔴 Red | Immediate attention required |

**Four protection areas:**

| Area | Purpose |
|------|---------|
| Virus & Threat Protection | Antivirus, malware scanning, ransomware protection |
| Firewall & Network Protection | Network traffic control per profile |
| App & Browser Control | SmartScreen and exploit protection |
| Device Security | Hardware-level security (TPM, Core Isolation) |

---

## Virus & Threat Protection

### Scan Options

| Scan Type | Description |
|-----------|-------------|
| Quick Scan | Checks common threat locations only |
| Full Scan | Scans all files and running programs — can take 1+ hour |
| Custom Scan | User-defined files and folders |

### Threat History
- **Quarantined threats** — Isolated and prevented from running
- **Allowed threats** — Items identified as threats but manually allowed

### Key Settings

| Setting | Purpose |
|---------|---------|
| Real-time protection | Continuously monitors for malware |
| Cloud-delivered protection | Faster detection using cloud threat data |
| Automatic sample submission | Sends suspicious files to Microsoft |
| Controlled folder access | Blocks unauthorized apps from modifying protected folders |
| Exclusions | Exempt specific files/folders from scanning (use with caution) |

### Ransomware Protection
- Requires **Controlled folder access** to be enabled
- Which requires **Real-time protection** to be enabled



---

## Firewall & Network Protection

A firewall controls what network traffic is allowed in and out of the system — think of it as a security guard checking IDs at the door.

**Three Windows Firewall profiles:**

| Profile | When It Applies |
|---------|----------------|
| Domain | Machine is connected to a corporate domain network |
| Private | Home or trusted private network |
| Public | Untrusted networks — airports, coffee shops, hotels |

> **If connected to airport Wi-Fi, the active firewall profile would be: Public**

**Command to open Windows Defender Firewall advanced settings:**
```
WF.msc
```

Each profile can be individually configured to:
- Turn the firewall on or off
- Block all incoming connections

> ⚠️ Do not disable the firewall unless you have a third-party firewall solution in place.

---

## App & Browser Control — SmartScreen

**Microsoft Defender SmartScreen** protects against:
- Phishing websites
- Malware websites and applications
- Downloading potentially malicious files

**SmartScreen settings:**

| Setting | Behavior |
|---------|----------|
| Block | Blocks access completely |
| Warn | Warns the user but allows them to proceed |
| Off | No protection |

**Exploit Protection** is also configured here — built into Windows 10/Server 2019 to protect against memory exploitation attacks.

---

## Device Security

### Core Isolation — Memory Integrity
- Prevents attackers from inserting malicious code into high-security processes
- Uses virtualization-based security to isolate critical processes

### Trusted Platform Module (TPM)

**TPM** is a hardware chip built into modern computers that provides:
- Hardware-based cryptographic functions
- Tamper-resistant security
- Secure storage of encryption keys

> **TPM full name:** Trusted Platform Module

TPM is required for:
- BitLocker full protection
- Windows Hello
- Certain enterprise security features

---

## BitLocker

**BitLocker Drive Encryption** protects data on lost, stolen, or decommissioned devices by encrypting the entire drive.

**Best protection:** BitLocker + TPM version 1.2 or later working together.

**On systems without TPM:** A **USB startup key** (removable drive) is used instead — the drive contains the startup key needed to unlock the encrypted volume.

> This is why BitLocker is only available on Windows **Pro** edition (not Home).

---

## Volume Shadow Copy Service (VSS)

**VSS** creates consistent point-in-time snapshots (shadow copies) of data for backup purposes.

Shadow copies are stored in the **System Volume Information** folder on each protected drive.

**With VSS enabled, you can:**
- Create restore points
- Perform system restore
- Configure restore settings
- Delete restore points

**Security significance — Ransomware:**  
Ransomware specifically targets and **deletes shadow copies** as part of its attack chain to prevent victims from recovering their files without paying the ransom. This makes offline/offsite backups the only reliable recovery option after a ransomware attack.

> **VSS stands for:** Volume Shadow Copy Service

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| Date the two definition updates were installed? | 5/3/2021 |
| Which area in Windows Security needs immediate attention? | Virus & threat protection |
| What is turned off that Windows is notifying about? | Real-time protection |
| Active firewall profile at airport Wi-Fi? | Public |
| What is TPM? | Trusted Platform Module |
| What does a removable drive contain for BitLocker on systems without TPM? | Startup key |
| What is VSS? | Volume Shadow Copy Service |

---

## How This Connects to My SOC Work

This room directly maps to the defensive tools I monitor and investigate at TPSODL:

- **Windows Defender / Real-time protection** — Our endpoints rely on Defender as the primary AV solution. When Defender generates alerts that feed into ArcSight, I triage them to determine if they are true positives. Understanding Defender's scan types and quarantine behavior helps me assess alert severity accurately.
- **Firewall profiles** — During lateral movement investigations, checking which firewall profile is active and what rules are in place on an endpoint helps determine if an attacker could have moved laterally via SMB, RDP, or other protocols.
- **SmartScreen** — Phishing and drive-by download alerts often involve SmartScreen being bypassed or disabled. Seeing this turned off on an endpoint is a red flag.
- **BitLocker** — In ransomware investigations, confirming whether BitLocker was active before encryption helps determine the scope of data exposure on decommissioned or stolen drives.
- **VSS / Shadow Copies** — When we respond to ransomware incidents, the first thing we check is whether shadow copies were deleted. This determines whether a restore-point-based recovery is possible or whether we need to fall back to backups.
- **TPM** — Device security posture checks in our environment include verifying TPM status on endpoints. A missing or disabled TPM is a risk indicator.

---

## Key Takeaway

Windows has a robust set of built-in security tools — but they only protect you if they are properly configured and actively monitored. A SOC analyst needs to understand not just what these tools do, but how attackers disable or bypass them. Real-time protection being turned off, firewall profiles being switched, shadow copies being deleted — these are all indicators of compromise that show up in logs and alerts.
