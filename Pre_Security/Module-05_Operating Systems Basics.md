# Module - 05
# Operating Systems Basics — TryHackMe Pre Security

## Rooms Covered
- Operating Systems: Introduction
- Windows Basics
- Linux CLI Basics
- Windows CLI Basics
- Operating System Security

---

## Operating Systems: Introduction

An Operating System (OS) is the core software layer that sits between
the hardware and the applications we use. Without it, every app would
need to directly control the CPU, memory, and devices — causing chaos.

**The Airport Analogy:**
- Hardware = runways, fuel systems, radar infrastructure
- Applications = airlines and passengers requesting services
- OS = the air traffic control system managing everything

**System Privilege Layers:**
- `Kernel Space` — privileged zone with unrestricted hardware access;
  only the kernel runs here
- `User Space` — where all applications run with restricted permissions;
  they communicate with the kernel via system calls

**Core OS Responsibilities:**

| Responsibility | What It Does |
|----------------|-------------|
| Process Management | Schedules and terminates running programs |
| Memory Management | Allocates RAM, uses virtual memory when needed |
| File System Management | Organizes files, handles permissions and metadata |
| User Management | Handles accounts, authentication, access control |
| Device Management | Loads drivers; provides hardware abstraction layer |

**OS Types Encountered in the Real World:**
- Desktop — Windows, macOS, Linux (Ubuntu, Fedora)
- Server — Windows Server, Ubuntu Server, Red Hat
- Mobile — Android, iOS
- Embedded/IoT — FreeRTOS, OpenWrt
- Virtual/Cloud — Alpine Linux, Amazon Linux

**Practical — About This Computer (Ubuntu MATE):**
- Ubuntu MATE version: `1.26.2`
- RAM allocated: `1.9 GiB`
- `/dev/root` filesystem type: `ext4`

---

## Windows Basics

Windows is the most widely used desktop OS. This room covered
navigating the Windows environment and using its built-in tools.

**Key Windows Interface Components:**
- `Desktop` — main workspace with shortcuts and folders
- `Taskbar` — quick access to pinned apps, clock, notifications
- `Start Menu` — central access point for apps, settings, power
- `File Explorer` — GUI tool to browse and manage files/folders
- `Task Manager` — real-time monitor for processes, CPU, RAM, users

**Windows Security Tools:**
- `Windows Security` — central dashboard for all built-in protections
- `Windows Defender Firewall` — controls inbound/outbound traffic
  across three profiles: Domain, Private, Public
- `Virus & Threat Protection` — real-time malware scanning;
  custom scans can target specific folders

**Windows Settings vs Control Panel:**
- Settings app — modern, centralized configuration interface
- Control Panel — legacy tool still needed for specific admin tasks

**Practical — TryHatMe Onboarding Lab:**
- Device name: `TryHatMe`
- RAM: `4.00 GB`
- Windows version: `1809` (Server 2019 Datacenter)
- Installed and ran `TryHatMeWelcome` application
- Custom security scan detected: `Virus:DOS/EICAR_Test_File`

---

## Linux CLI Basics

Linux is the dominant OS in cybersecurity environments — servers,
SIEM backends, security tools, and attacker machines all run on it.
This room introduced terminal navigation as a first-day intern
on a Cyber Operations Support Team.

**Core Navigation Commands:**

```bash
pwd              # Print working directory — where am I?
ls               # List directory contents
ls -l            # Long format with permissions, size, dates
ls -al           # Include hidden files (files starting with .)
cd Documents/    # Change into a directory
cd ..            # Move one level up
find ~ -name mission_brief.txt   # Search for a file by name
cat mission_brief.txt            # Read file contents
```

**System Information Commands:**

```bash
whoami           # Current logged-in username
uname -a         # Full system info: kernel, hostname, architecture
df -h            # Disk usage in human-readable format
cat /etc/os-release   # Detailed Linux distro information
```

**Key Concepts:**
- Hidden files start with `.` and are invisible by default with `ls`
- `/etc` directory stores system configuration files
- `find` recursively searches from a starting path

**Practical — Mission Lab Results:**
- Full path of `mission_brief.txt`:
  `/home/ubuntu/Documents/.research/archive/mission_brief.txt`
- Kernel version: `6.14.0-1018-aws`
- Free disk space: `58G`
- `whoami` output: `ubuntu`

---

## Windows CLI Basics

CMD (Command Prompt) is the Windows equivalent of the Linux terminal.
This room continued the internship storyline on Day 2, using CMD to
navigate files and gather system information.

**Core Navigation Commands:**

```cmd
cd                    # Show current directory path
dir                   # List contents of current directory
dir /a                # Show all files including hidden ones
dir /s task_brief.txt # Search for file across all subfolders
cd Documents          # Navigate into a folder
type task_brief.txt   # Read and print file contents
```

**System Information Commands:**

```cmd
whoami       # Current logged-in user
hostname     # Name of the computer
systeminfo   # Full OS details: version, type, memory
ipconfig     # Network config: IP address, default gateway
```

**Practical — Day 2 Lab Results:**
- Full path of `task_brief.txt`:
  `C:\Users\Administrator\Documents\Notes\research_yn6\exports_imv\screenshots\notes_wi6`
- Computer name returned by `hostname`: checked via `systeminfo`

---

## Operating System Security

This room tied OS knowledge directly into security — covering the three
weaknesses attackers most commonly exploit.

**CIA Triad in the OS Context:**
- `Confidentiality` — who can read your files?
- `Integrity` — who can modify your files?
- `Availability` — can you access your files at all?

**Three Key Attack Vectors:**

**1. Authentication & Weak Passwords**
- Attackers exploit predictable passwords (123456, dragon, qwerty)
- Common patterns: keyboard walks (`1q2w3e4r5t`), personal info,
  dictionary words
- Defence: complex, unique passwords per account

**2. Weak File Permissions**
- Principle of Least Privilege — users should only access what
  they need
- Weak permissions attack both confidentiality and integrity

**3. Malicious Programs**
- Trojan Horse — hides inside a legitimate-looking program to give
  attacker system access
- Ransomware — encrypts files and demands payment to restore access

**Practical — SSH Privilege Escalation Lab:**

```bash
ssh sammie@MACHINE_IP   # Login with discovered credentials
whoami                  # Confirm logged-in user
ls                      # List files in home directory
cat draft.md            # Read file contents
history                 # View command history (found root password)
su - root               # Switch to root using leaked password
cat /root/flag.txt      # Read root flag
```

- `sammie` password found via sticky note: `dragon`
- `johnny` password guessed from top passwords list: `abc123`
- Root password leaked via `johnny`'s command history: `happyHack!NG`
- Root flag: `THM{YouGotRoot}`

---

## How This Connects to My SOC Work

Every alert I triage in ArcSight SIEM originates from an OS — Windows
endpoints generating Event IDs, Linux servers producing auth logs,
embedded devices sending syslog. This module finally gave me the
foundation to understand *what* those systems are doing under the hood.

The privilege separation concept (kernel vs user space) directly maps
to how I investigate privilege escalation alerts in our SOC. When an
endpoint triggers a high-severity alert for unexpected `su` or
`runas` activity, I now understand exactly what that means at the OS
level — a process trying to move from user space permissions to
kernel-level control.

The OS Security room was the most directly applicable. The SSH lab
mirrored real lateral movement scenarios — credential reuse,
command history leaking sensitive data, and weak passwords enabling
privilege escalation. These are patterns I see in alerts daily.

The `whoami`, `hostname`, and `systeminfo` commands are also part of
standard Windows incident triage — the first thing an analyst runs
when RDP-ing into a compromised machine.

---

## Key Takeaway

The OS is not just a convenience layer — it is the first line of
defence and the primary attack surface. Understanding how it manages
users, permissions, processes, and files is not optional for a SOC
analyst. Every alert, every log line, and every investigation starts
at the operating system level.