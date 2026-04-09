# Room 03: Search Skills
# TryHackMe — Cyber Security 101
# Module -01 Start Your Cyber Security Journey

**Platform:** TryHackMe  
**Path:** Cyber Security 101  
**Difficulty:** Easy  
**Status:** Completed ✅

---

## Why Search Skills Matter in Cybersecurity

A search for "learn hacking" returns over **1.5 billion results**. The challenge is not finding information — it's finding the **right** information efficiently and evaluating whether it's trustworthy.

For a cybersecurity professional, strong search skills directly impact your ability to:
- Investigate incidents and look up IOCs quickly
- Find CVEs and exploits for vulnerability assessments
- Stay updated on emerging threats and TTPs
- Look up technical documentation during live investigations

---

## Evaluating Information Sources

Not everything on the internet is accurate or trustworthy. Before acting on information, evaluate it using these criteria:

| Criteria | What to Ask |
|----------|-------------|
| **Source** | Who wrote this? Are they reputable and authoritative? |
| **Evidence & Reasoning** | Are claims backed by facts and logical arguments? |
| **Objectivity & Bias** | Is the author pushing an agenda or promoting a product? |
| **Corroboration** | Do multiple independent, reliable sources agree? |

> ⚠️ Publishing a blog post does not make someone an authority. Always verify.

**Key term:** A cryptographic method or product that is bogus or fraudulent is called **snake oil**.

---

## Search Engine Operators

Search engines support advanced operators that help you find exactly what you need. These are essential for OSINT (Open Source Intelligence) and research.

**Google Advanced Operators:**

| Operator | Purpose | Example |
|----------|---------|---------|
| `"exact phrase"` | Find pages with exact wording | `"passive reconnaissance"` |
| `site:` | Limit results to a specific domain | `site:tryhackme.com success stories` |
| `-` | Exclude a word from results | `pyramids -tourism` |
| `filetype:` | Find specific file types | `filetype:ppt cyber security` |

**Example:** To find PDF files about cyber warfare reports:
```
filetype:pdf cyber warfare report
```

---

## Specialized Search Engines

Beyond Google, cybersecurity professionals rely on purpose-built search tools:

### 🔍 Shodan — `shodan.io`
Search engine for **internet-connected devices**. Finds servers, routers, webcams, ICS/SCADA systems, and IoT devices exposed to the internet.

*Use case:* Check how many servers are still running a vulnerable version of Apache.

### 🔍 Censys — `censys.io`
Focuses on **internet-connected hosts, websites, and certificates**. Useful for:
- Enumerating domains in use
- Auditing open ports and services
- Discovering rogue assets on a network

### 🔍 VirusTotal — `virustotal.com`
Scans files, URLs, and file hashes against **multiple antivirus engines simultaneously**.

*Use case in SOC:* Submit a suspicious file hash from an alert to quickly check if it's known malware.

### 🔍 Have I Been Pwned — `haveibeenpwned.com`
Checks if an **email address has appeared in a data breach**. Critical for understanding credential exposure risk.

### 🔍 CVE Program — `cve.mitre.org` / NVD — `nvd.nist.gov`
A standardized **dictionary of known vulnerabilities**. Every vulnerability gets a unique CVE ID (e.g., `CVE-2024-29988`).

*CVE-2024-3094 refers to:* **XZ Utils** (a critical backdoor in a Linux compression utility)

### 🔍 Exploit Database — `exploit-db.com`
A public archive of **exploit code** for known vulnerabilities. Used by penetration testers to find working exploits for authorized testing.

### 🔍 GitHub — `github.com`
Often contains **PoC (Proof of Concept)** code and tools related to CVEs. Useful for both offense (finding exploits) and defense (understanding how vulnerabilities work).

---

## Technical Documentation

Knowing how to read official documentation is a core skill.

### Linux Man Pages
Every Linux command has a manual page accessible via:
```bash
man <command>
```
Example: `man ip` shows the full reference for the `ip` command.

- **`cat`** stands for: **concatenate**
- The command replacing `netstat` on Linux: **`ss`** (Socket Statistics)

### Microsoft Windows Documentation
Available at Microsoft's official Technical Documentation site. Useful for commands like `ipconfig`, `netstat`, etc.

**`netstat` parameter in Windows to show executables:**
```
netstat -b
```

### Product Documentation
Always check official docs first — they are the most accurate and up-to-date source. Examples:
- Snort, Apache HTTP Server, PHP, Node.js all have official documentation portals.

---

## Social Media for Cybersecurity

Social media is a powerful tool for both **OSINT investigations** and **professional development**.

**For investigations:**
- **LinkedIn** — Research the technical background of employees at a target company (used in red team reconnaissance)
- **Facebook** — Find answers to security questions like "Which school did you go to as a child?" (used in social engineering risk assessments)

**For staying updated:**
- Follow cybersecurity researchers, vendors, and threat intelligence groups on Twitter/X and LinkedIn
- Subscribe to relevant communities to stay ahead of emerging threats

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| How many results for "learn hacking" on Google? | ~1.5 billion |
| What do you call a fraudulent cryptographic product? | Snake oil |
| Linux command replacing `netstat`? | `ss` |
| How to limit Google search to PDF files with "cyber warfare report"? | `filetype:pdf cyber warfare report` |
| What does `ss` stand for? | Socket Statistics |
| What does the Linux command `cat` stand for? | Concatenate |
| `netstat` parameter in Windows showing executables? | `-b` |
| What does CVE-2024-3094 refer to? | XZ Utils |
| Social media for researching employee background? | LinkedIn |
| Social media for finding answers to secret questions? | Facebook |

---

## How This Connects to My SOC Work

Search skills are something I use every single day in the SOC at TPSODL.

- **VirusTotal** — I regularly submit file hashes and IPs from ArcSight alerts to VirusTotal to quickly validate if they are known IOCs
- **CVE lookups** — When a vulnerability alert fires, I check NVD/CVE for CVSS score and impact to prioritize the response
- **Shodan** — Useful during threat hunting to check if any of our public-facing assets have unintended exposure
- **Exploit-DB** — Helps me understand the technical details of vulnerabilities so I can better assess alert severity

This room reinforced that **OSINT and research skills are not optional** — they are fundamental to everything a SOC analyst does.

---

## Key Takeaway

In cybersecurity, your ability to find the right information quickly is as important as your technical knowledge. Knowing which tool to use — Google operators, Shodan, VirusTotal, CVE databases — and how to evaluate what you find separates a good analyst from a great one.
