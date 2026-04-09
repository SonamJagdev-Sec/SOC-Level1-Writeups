# Room 02: Defensive Security Intro
# TryHackMe — Cyber Security 101

## What is Defensive Security?

Defensive security focuses on **protecting** systems and networks from attackers. It has two core goals:

1. **Preventing** intrusions from occurring
2. **Detecting and responding** to intrusions when they do occur

The teams responsible for this are called **Blue Teams**. They are the defenders — the counterpart to the offensive red teams.

---

## Core Defensive Tasks

- **User Cyber Security Awareness** — Training users to avoid falling for phishing and social engineering
- **Asset Management** — Knowing and documenting every device and system you are responsible for protecting
- **Patching & Updates** — Keeping systems updated to close known vulnerabilities
- **Preventative Security Devices** — Deploying firewalls and IPS (Intrusion Prevention Systems) to block malicious traffic
- **Logging & Monitoring** — Ensuring all activity is logged so suspicious behavior can be detected

---

## Areas of Defensive Security

### 🔵 SOC — Security Operations Center

A SOC is a team of cybersecurity professionals who **monitor** the network 24/7 for malicious activity.

**What a SOC monitors for:**
- **System Vulnerabilities** — Unpatched weaknesses that could be exploited
- **Policy Violations** — Users breaking security rules (e.g., uploading confidential data externally)
- **Unauthorized Activity** — Compromised credentials being used by an attacker
- **Network Intrusions** — Attackers gaining access through malicious links or exposed services

---

### 🔵 Threat Intelligence

Threat Intelligence is the process of **collecting and analyzing information** about actual and potential adversaries to build a **threat-informed defence**.

**The process:**
1. **Collect** — Gather data from network logs, public forums, threat feeds
2. **Process** — Organize the raw data into a usable format
3. **Analyze** — Identify attacker TTPs (Tactics, Techniques, and Procedures), build a list of recommended actions

The goal: know your adversary before they attack you.

---

### 🔵 DFIR — Digital Forensics and Incident Response

#### Digital Forensics

Digital forensics is the application of science to **investigate cyberattacks and establish facts**. Key areas:

| Area | What It Covers |
|------|----------------|
| File System | Installed programs, created/deleted/overwritten files |
| System Memory | Malware running in RAM without touching disk |
| System Logs | Timeline of events on client/server machines |
| Network Logs | Traffic analysis to understand what happened over the network |

#### Incident Response

An **incident** is any cybersecurity event — a breach, intrusion attempt, policy violation, or attack.

**The 4 Phases of Incident Response:**

| Phase | Description |
|-------|-------------|
| 1. Preparation | Train the team, put preventive measures in place |
| 2. Detection & Analysis | Detect the incident and assess its severity |
| 3. Containment, Eradication & Recovery | Stop the spread, remove the threat, restore systems |
| 4. Post-Incident Activity | Write a report, share lessons learned, improve defences |

---

### 🔵 Malware Analysis

**Malware** (malicious software) is any program designed to harm a system or its users.

**Types of malware covered:**

| Type | Description |
|------|-------------|
| Virus | Attaches to programs, spreads between systems, damages/deletes files |
| Trojan Horse | Appears legitimate but hides malicious functionality |
| Ransomware | Encrypts files and demands payment for decryption |

**Two approaches to analyzing malware:**

- **Static Analysis** — Inspecting the malware *without* running it; requires knowledge of assembly language
- **Dynamic Analysis** — Running the malware in a controlled sandbox and observing its behavior

---

## Lab: SIEM Simulation

The room included a hands-on simulation of a **SIEM (Security Information and Event Management)** system — the same kind of tool used in real SOC environments.

**What I did:**
- Reviewed security alerts generated in the simulated SIEM dashboard
- Analyzed events to determine which were malicious vs. false positives
- Investigated alerts related to failed logins and unknown IP connections
- Located the flag by following the alert investigation workflow

**Key insight:** Not every alert is malicious. A SOC analyst's job is to use context and expertise to distinguish **true positives** from **false positives**.

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| What would you call a team that monitors a network for malicious events? | Security Operations Center |
| What does DFIR stand for? | Digital Forensics and Incident Response |
| Which kind of malware requires the user to pay money to regain access to their files? | Ransomware |
| Which team focuses on defensive security? | Blue Team |
| What is the flag obtained from the SIEM simulation? | *(obtained during lab)* |

---

## How This Connects to My SOC Work

This entire room is essentially a description of my daily work at TPSODL as a SOC analyst.

- **SIEM simulation** → I work in **ArcSight SIEM** daily, triaging alerts and validating IOCs
- **Incident Response phases** → These directly match our internal SOC runbooks — from detection through post-incident reporting
- **Threat Intelligence** → We consume threat feeds and use them to tune detection rules and validate suspicious IPs/hashes
- **False positive vs. true positive** → This is the core decision I make on every alert I handle

The lab made me realize how well-designed TryHackMe's simulation is — it genuinely mirrors real SOC operations at a foundational level.

---

## Key Takeaway

Defensive security is not passive — it requires deep technical knowledge, analytical thinking, and the ability to act fast under pressure. The SOC is the frontline of cyber defence, and tools like SIEM are its eyes and ears.
