# Room 02: Windows Fundamentals 2
# TryHackMe — Cyber Security 101 | Module 03: Windows and AD Fundamentals


## Overview

Building on Windows Fundamentals 1, this room dives into the system utilities available through the **MSConfig (System Configuration)** panel. These tools are essential for troubleshooting, monitoring, and understanding what's running on a Windows system — all critical skills for a SOC analyst.

---

## System Configuration — MSConfig

**Command to open:** `msconfig`  
**Purpose:** Advanced troubleshooting tool, primarily for diagnosing startup issues.

> ⚠️ Requires local administrator rights.

**Five tabs in MSConfig:**

| Tab | Purpose |
|-----|---------|
| General | Choose startup mode: Normal, Diagnostic, or Selective |
| Boot | Configure OS boot options |
| Services | List all configured services (running or stopped) |
| Startup | Manage startup items (redirects to Task Manager on Windows 10/11) |
| Tools | Quick launch shortcuts to various system utilities |

**Key finding from the lab:**
- Service with **Sysinternals** as manufacturer: **PsShutdown**
- Windows license registered to: **Windows User**
- Command for Windows Troubleshooting: `C:\Windows\System32\control.exe /name Microsoft.Troubleshooting`
- Command to open Control Panel: `control.exe`

---

## Computer Management — compmgmt.msc

**Command:** `compmgmt.msc`  
Three primary sections: **System Tools**, **Storage**, and **Services and Applications**

### System Tools

**Task Scheduler**
- Create and manage automated tasks that run at specified times or triggers
- Tasks can run scripts, executables, or applications
- Trigger types: at login, at logoff, on a schedule, at system startup
- **Lab finding:** `npcapwatchdog` scheduled task runs at **system startup**

**Event Viewer**
- View and analyze event logs on the system — essentially the audit trail of Windows
- Three panes: log providers (left), event details (center), actions (right)

**Five event types:**

| Type | Description |
|------|-------------|
| Error | Significant problem (e.g., service failure) |
| Warning | Not immediately significant but may indicate future problems |
| Information | Successful operation of an app, driver, or service |
| Success Audit | Audited security access that succeeded |
| Failure Audit | Audited security access that failed |

**Standard Windows Logs:**

| Log | Contents |
|-----|---------|
| Application | App-related events |
| Security | Login attempts, privilege use, audit policy changes |
| Setup | System setup events |
| System | OS component events |
| Forwarded Events | Events collected from remote machines |

**Shared Folders**
- View all network shares on the system
- Default admin shares: `C$`, `ADMIN$`
- Monitor active sessions and open files

**Performance Monitor (perfmon)**
- View real-time or historical performance data
- Useful for diagnosing performance issues locally or remotely

**Device Manager**
- View and configure hardware devices
- Can disable/enable hardware attached to the computer

### Storage — Disk Management

Perform advanced storage tasks:
- Set up new drives
- Extend or shrink partitions
- Assign or change drive letters

### Services and Applications

**Services**
- List all services with display name, status, and startup type
- Startup types: **Automatic**, **Manual**, **Disabled**

**WMI Control**
- Configures Windows Management Instrumentation (WMI)
- Allows scripting languages (PowerShell, VBScript) to manage Windows systems locally and remotely
- Note: `WMIC` command-line tool is deprecated in Windows 10 v21H1 — PowerShell is the replacement

---

## UAC Settings

UAC settings can be modified through the **User Account Control Settings** panel.

**Command:** `UserAccountControlSettings.exe`

Four security levels (slider-based):

| Level | Behavior |
|-------|----------|
| Always notify | Highest security — prompts for everything |
| Notify for apps (default) | Prompts only when apps make changes |
| Notify without dimming | Same as above, no Secure Desktop |
| Never notify | No protection — not recommended |

---

## System Information — msinfo32

**Command:** `msinfo32.exe`  
Provides a comprehensive view of hardware, system components, and software environment.

**Three sections:**

| Section | Contents |
|---------|---------|
| Hardware Resources | IRQs, DMA, memory addresses — advanced use |
| Components | Hardware details: display, input, network, storage |
| Software Environment | Environment variables, running tasks, network connections |

**Key environment variable:**  
`ComSpec` = `%SystemRoot%\system32\cmd.exe`

---

## Resource Monitor — resmon

**Command:** `resmon.exe`  
Displays real-time and logged performance data per-process and in aggregate.

**Four sections:**

| Tab | What It Shows |
|-----|--------------|
| CPU | Per-process CPU usage, handles, modules |
| Memory | Physical memory usage and allocation |
| Disk | Read/write activity per process |
| Network | Network activity per process |

Useful for identifying deadlocked processes, file locking conflicts, and abnormal resource consumption.

---

## Command Prompt — cmd

The command line interface for Windows. Essential for both administration and security investigations.

**Key commands covered:**

| Command | Purpose |
|---------|---------|
| `hostname` | Display the computer name |
| `whoami` | Display the current logged-in user |
| `ipconfig` | Show network address settings |
| `ipconfig /all` | Show detailed network info |
| `netstat` | Display protocol stats and active TCP/IP connections |
| `net` | Manage network resources (requires sub-commands) |
| `cls` | Clear the command prompt screen |

**Help syntax:**
- For most commands: `command /?`
- For `net` commands: `net help <subcommand>` (e.g., `net help user`)

**Full command for Internet Protocol Configuration (from MSConfig):**  
`C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe`

---

## Registry Editor — regedit

**Command:** `regedit.exe`  
The Windows Registry is a central hierarchical database storing configuration for:
- User profiles
- Installed applications
- Hardware devices
- System ports and settings

> ⚠️ The registry is for advanced users only. Incorrect changes can break Windows.

---

## Advanced System Settings

**Page File** — Virtual memory used when physical RAM is full. Viewable at:  
`Advanced System Settings → Performance → Settings → Advanced`

**Crash Dump Types:**

| Type | Description |
|------|-------------|
| Automatic memory dump | Windows decides what to capture |
| Kernel memory dump | Kernel memory at time of crash |
| Small memory dump (256 KB) | Minimal info — just the stop message |
| Complete memory dump | Full RAM contents |
| None | No dump created |

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| Service with Sysinternals as manufacturer? | PsShutdown |
| Windows license registered to? | Windows User |
| Command for Windows Troubleshooting? | C:\Windows\System32\control.exe /name Microsoft.Troubleshooting |
| Command to open Control Panel (.exe name)? | control.exe |
| Command to open Computer Management? | compmgmt.msc |
| npcapwatchdog scheduled task runs when? | At system startup |
| Command to open UAC Settings (.exe name)? | UserAccountControlSettings.exe |
| Command to open System Information (.exe name)? | msinfo32.exe |
| Value for ComSpec environment variable? | %SystemRoot%\system32\cmd.exe |
| Command to open Resource Monitor (.exe name)? | resmon.exe |
| Full command for ipconfig in MSConfig? | C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe |
| How to show detailed info with ipconfig? | ipconfig /all |
| Command to open Registry Editor (.exe name)? | regedit.exe |

---

## How This Connects to My SOC Work

This room covers tools I use directly in incident response and threat hunting at TPSODL:

- **Event Viewer** — My primary tool for log analysis on Windows endpoints. Security logs (Event ID 4624, 4625, 4648, etc.) are the first place I look when investigating unauthorized access alerts from ArcSight.
- **Task Scheduler** — A common persistence mechanism used by malware. Attackers create scheduled tasks to maintain access after reboot. I check Task Scheduler on suspicious machines during investigations.
- **Services** — Many malware families install themselves as Windows services for persistence. Reviewing services with unusual names, paths, or manufacturers is a standard part of endpoint triage.
- **Resource Monitor** — Useful for live endpoint analysis to spot processes with abnormal network connections or disk activity.
- **Registry Editor** — Another critical persistence location. Attackers add keys to `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` for startup persistence. This is something I check during malware investigations.
- **WMI** — Increasingly abused by fileless malware and APTs for lateral movement and persistence. Understanding WMI is essential for detecting LOLBin (Living Off the Land) attacks.

---

## Key Takeaway

MSConfig and the tools it launches are not just administrative utilities — they are an investigator's toolkit. Every tool in this room is something an attacker either abuses for persistence/lateral movement or something a defender uses to detect and respond to those actions. Knowing both sides makes you a stronger analyst.
