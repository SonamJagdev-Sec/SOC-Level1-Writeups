Linux Fundamental :
# Linux Fundamentals — TryHackMe Pre Security
#TryHackMe — Cyber Security 101

## Rooms Covered
- Linux Fundamentals Part 1
- Linux Fundamentals Part 2
- Linux Fundamentals Part 3

---

## Part 1 — Introduction & Basic Commands

**What is Linux?**
- Open-source operating system based on UNIX
- Powers websites, servers, smart cars, Android devices,
  supercomputers, critical infrastructure
- Common distributions: Ubuntu, Debian, Kali Linux
- Ubuntu Server can run on just 512MB RAM

**Essential Basic Commands:**

| Command | Full Name | Purpose |
|---------|-----------|---------|
| `echo` | echo | Print text to terminal |
| `whoami` | whoami | Show current logged-in user |
| `ls` | listing | List files in current directory |
| `cd` | change directory | Navigate to a folder |
| `cat` | concatenate | Output contents of a file |
| `pwd` | print working directory | Show full current path |

**Shell Operators:**

| Operator | Purpose |
|----------|---------|
| `&` | Run command in background |
| `&&` | Run second command only if first succeeds |
| `>` | Redirect output to file (overwrites) |
| `>>` | Redirect output to file (appends) |

**Examples:**
```bash
echo hello > welcome       # creates file with "hello"
echo world >> welcome      # appends "world" to file
command1 && command2       # runs command2 only if command1 works
cp -R /home/user/docs /var/backups/  # copy folder
```

---

## Part 2 — SSH, Filesystem & Permissions

**SSH (Secure Shell):**
- Protocol for remotely connecting to Linux machines
- All data is encrypted during transmission
- Syntax: `ssh username@IP_address`
- Example: `ssh tryhackme@10.10.10.10`

**Filesystem Interaction Commands:**

| Command | Purpose | Example |
|---------|---------|---------|
| `touch` | Create new empty file | `touch note.txt` |
| `mkdir` | Create new directory | `mkdir myfolder` |
| `cp` | Copy file or folder | `cp file1 file2` |
| `mv` | Move or rename file | `mv file1 myfolder` |
| `rm` | Remove file | `rm file.txt` |
| `rm -R` | Remove directory | `rm -R myfolder` |
| `file` | Determine file type | `file unknown1` |

**File Permissions:**
- Every file has permissions for: Owner, Group, Others
- Permissions: Read (r=4), Write (w=2), Execute (x=1)

| Symbolic | Numeric | Meaning |
|----------|---------|---------|
| rwxrwxrwx | 777 | Full access for all |
| rwxr-xr-x | 755 | Owner full, others read/execute |
| rw-r--r-- | 644 | Owner read/write, others read only |
| rwx------ | 700 | Only owner has access |
```bash
chmod 755 script.sh     # set permissions
ls -lh                  # view permissions
su user2                # switch to user2
su -l user2             # switch with user2's environment
```

**Important Linux Directories:**

| Directory | Purpose |
|-----------|---------|
| `/etc` | System config files (passwd, shadow, sudoers) |
| `/var` | Variable data — logs stored in `/var/log` |
| `/root` | Home directory for root user |
| `/tmp` | Temporary files — cleared on reboot, writable by all |
| `/home` | Home directories for regular users |

---

## Part 3 — Advanced Utilities & Automation

**Text Editors:**
- **Nano** — beginner friendly
  - Open/create file: `nano filename`
  - Save: Ctrl+O | Exit: Ctrl+X | Search: Ctrl+W
- **VIM** — advanced, highly customisable, syntax highlighting

**Process Management:**

| Command | Purpose |
|---------|---------|
| `ps` | View running processes for current session |
| `ps aux` | View all running processes (all users) |
| `top` | Real-time process monitoring |
| `kill PID` | Terminate a process by PID |

**Kill Signals:**
- `SIGTERM` — graceful kill (allows cleanup)
- `SIGKILL` — force kill immediately
- `SIGSTOP` — pause/suspend process

**Systemctl — Managing Services:**
```bash
systemctl start apache2     # start service
systemctl stop apache2      # stop service
systemctl enable apache2    # start on boot
systemctl disable apache2   # don't start on boot
systemctl status apache2    # check service status
```

**Crontabs — Task Automation:**
- Used to schedule tasks automatically
- Edit with: `crontab -e`
- Format: `MIN HOUR DOM MON DOW CMD`
```bash
# Example: backup Documents every 12 hours
0 */12 * * * cp -R /home/user/Documents /var/backups/

# Wildcard (*) = any value for that field
```

**Package Management (apt):**
```bash
apt update              # update package list
apt install <package>   # install software
apt remove <package>    # remove software
```

**Log Files (`/var/log`):**
- Apache2 logs — web server access and errors
- fail2ban logs — brute force monitoring
- UFW logs — firewall activity
- Two main log types:
  - `access.log` — all requests made to server
  - `error.log` — errors encountered by server

---

## How This Connects to My SOC Work

Linux is central to my daily SOC operations at TPSODL:

- Most of our security tools (ArcSight SIEM, Netskope,
  TrendMicro DDI) run on Linux servers — basic navigation
  commands like ls, cd, cat, pwd are used constantly
- Log analysis is a core SOC task — all our security
  logs are stored in `/var/log` equivalent paths and
  reviewed daily for threat detection
- Process management (ps aux, kill, top) is used when
  investigating suspicious processes on endpoints
- File permissions knowledge is critical for VAPT
  remediation — misconfigured permissions (777) are
  common vulnerabilities we track and fix
- Crontabs are relevant to threat hunting — attackers
  often use cron jobs for persistence on compromised
  Linux systems
- SSH knowledge is essential — we use SSH to connect
  to remote servers for incident investigation
- Understanding /etc/passwd and /etc/shadow helps in
  analysing credential-based attacks and privilege
  escalation attempts

## Key Takeaway
Linux is the foundation of most cybersecurity tools
and server environments. A SOC analyst who is
comfortable with Linux commands, file permissions,
process management and log analysis will be
significantly more effective in threat detection
and incident response.