# Room 01: Windows Fundamentals 1
# TryHackMe — Cyber Security 101 | Module 03: Windows and AD Fundamentals


## Overview

Windows is the most dominant operating system in both home and corporate environments, which makes it a primary target for attackers. Understanding how Windows works at a fundamental level is essential for any cybersecurity professional — whether you're defending it or attacking it.

This room covers the Windows GUI, file system, user accounts, UAC, and core system tools.

---

## Windows OS — Brief History

| Version | Notes |
|---------|-------|
| Windows XP | Long-running, widely used, now end-of-life |
| Windows Vista | Major overhaul, poorly received, quickly phased out |
| Windows 7 | Stable and widely adopted after XP |
| Windows 8/8.x | Short-lived, touch-screen focused |
| Windows 10 | Current mainstream desktop OS |
| Windows 11 | Current latest desktop OS (released Oct 2021) |
| Windows Server 2025 | Current Windows Server version |

> The VM in this room runs **Windows Server 2019 Standard**.

**Key difference — Home vs Pro:**  
Windows Pro includes **BitLocker** encryption, which is not available in the Home edition.

---

## The Windows Desktop (GUI)

The Windows Desktop is the graphical interface presented after login. Key components:

| Component | Description |
|-----------|-------------|
| Desktop | Workspace for shortcuts, files, folders |
| Start Menu | Access to apps, settings, power options |
| Search Box (Cortana) | Search for files, apps, and settings |
| Task View | Switch between open windows and virtual desktops |
| Taskbar | Shows running/pinned apps |
| Notification Area | Clock, network, volume, Action Center icons |

**Notification Area** — besides Clock and Network, the other visible icon is: **Action Center**

**Search Box options:**
- Hidden → hides/disables the Search Box
- Show Task View button → hides/disables Task View

---

## The File System — NTFS

Modern Windows uses **NTFS (New Technology File System)** as its file system.

**Previous file systems:**
- FAT16/FAT32 — still used on USB drives, SD cards
- HPFS — older High Performance File System

**Why NTFS is better:**

| Feature | FAT32 | NTFS |
|---------|-------|------|
| Max file size | 4 GB | No practical limit |
| Permissions | ❌ | ✅ |
| Encryption (EFS) | ❌ | ✅ |
| Journaling (self-repair) | ❌ | ✅ |
| Compression | ❌ | ✅ |

**NTFS Permissions:**
- Full Control
- Modify
- Read & Execute
- List folder contents
- Read
- Write

**Alternate Data Streams (ADS):**
- A feature unique to NTFS — files can contain more than one data stream
- Not visible in Windows Explorer by default
- **Security relevance:** Malware writers use ADS to hide malicious data inside legitimate files
- Can be viewed using PowerShell

> **NTFS stands for:** New Technology File System  
> **System variable for the Windows folder:** `%windir%`

---

## Windows\System32 Folder

`C:\Windows\System32` is one of the most critical folders in the OS. It holds important system files and many built-in tools (cmd.exe, taskmgr.exe, etc.).

⚠️ **Warning:** Never delete files from System32. It can render Windows completely inoperational.

---

## User Accounts, Profiles & Permissions

Two types of local user accounts in Windows:

| Type | What They Can Do |
|------|-----------------|
| Administrator | Full system access — add/remove users, install software, change settings |
| Standard User | Limited to their own files/folders; cannot make system-level changes |

**User profiles** are stored at `C:\Users\<username>` and created automatically on first login.

Each profile contains default folders: Desktop, Documents, Downloads, Music, Pictures.

**Tool to manage local users and groups:** `lusrmgr.msc`

---

## User Account Control (UAC)

**UAC = User Account Control**

Introduced in Windows Vista to protect against unauthorized system changes — particularly by malware running in the context of a logged-in admin.

**How it works:**
- Even Administrator accounts don't run with full elevated privileges by default
- When an action requires elevated privileges, a UAC prompt appears
- The user must confirm (or enter credentials) before the action proceeds

**UAC Security Levels:**

| Level | Behavior |
|-------|----------|
| Always notify | Prompts for every change — most secure |
| Notify for apps (default) | Prompts only when apps make changes |
| Notify without dimming | Same as above but no Secure Desktop |
| Never notify | No prompts at all — least secure |

**Security significance:** UAC limits the blast radius of malware. Even if malware runs under an admin account, it cannot silently make system-level changes without triggering a UAC prompt.

---

## Settings vs Control Panel

| Tool | Purpose |
|------|---------|
| Settings | Modern interface, introduced in Windows 8, primary for most users |
| Control Panel | Legacy tool, used for more complex/advanced configurations |

Both are accessible from the Start Menu. Some Settings paths redirect to Control Panel for advanced options.

---

## Task Manager

Task Manager provides real-time information about:
- Running applications and processes
- CPU and RAM utilization
- Performance metrics
- Services

**Access methods:**
- Right-click the Taskbar → Task Manager
- Keyboard shortcut: `Ctrl + Shift + Esc`

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| Encryption you can enable on Pro but not Home? | BitLocker |
| What does NTFS stand for? | New Technology File System |
| System variable for the Windows folder? | %windir% |
| Selection to hide the Search Box? | Hidden |
| Selection to hide Task View button? | Show Task View button |
| Other icon in Notification Area besides Clock and Network? | Action Center |
| What does UAC stand for? | User Account Control |
| Last setting in Control Panel (Small icons view)? | Windows Defender Firewall |
| Keyboard shortcut to open Task Manager? | Ctrl + Shift + Esc |

---

## How This Connects to My SOC Work

As a SOC analyst at TPSODL, Windows fundamentals are the foundation of almost every investigation I conduct:

- **NTFS & ADS** — When investigating endpoint alerts, understanding how ADS works helps me recognize when malware is hiding data inside legitimate files. This is a known technique in fileless malware attacks.
- **User Accounts & UAC** — Many attack chains involve privilege escalation. Understanding the difference between standard and admin accounts and how UAC is bypassed is critical for alert triage.
- **Task Manager** — One of the first tools I'd use on a suspicious endpoint to check for rogue processes running alongside legitimate ones.
- **Event Logs / System32** — Most of the tools I rely on for forensic investigation (like `eventvwr.msc`) live in System32.

---

## Key Takeaway

Windows is the most attacked operating system in the world. Understanding its structure — file system, user model, UAC, and built-in tools — is not optional for a SOC analyst. Every escalation path, every persistence mechanism, and every lateral movement technique an attacker uses relies on abusing something covered in this room.
