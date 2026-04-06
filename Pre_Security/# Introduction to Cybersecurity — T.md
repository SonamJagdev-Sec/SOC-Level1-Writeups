# Introduction to Cybersecurity — TryHackMe Pre Security

## Rooms Covered
- Offensive Security Intro
- Defensive Security Intro
- Careers in Cyber

---
## Offensive Security
Offensive security is about thinking like an attacker 
to find and fix vulnerabilities before real hackers do.

**Key tool practiced:**
- `dirb` — brute-force tool to find hidden website pages
- Used dirb to discover hidden URL `/bank-deposit` 
  on a fake bank website

**Key concept:**
- Attackers look for hidden or exposed URLs
- Tools like dirb use wordlists to test predictable page names

---

## Defensive Security
Defensive security focuses on two main tasks:
- Preventing intrusions from occurring
- Detecting and responding to intrusions when they occur

**Key areas of defensive security:**
- SOC (Security Operations Center) — monitors network 
  for malicious events
- Threat Intelligence — collecting data about adversaries 
  to build threat-informed defence
- DFIR (Digital Forensics and Incident Response) — 
  investigating attacks and responding to incidents
- Malware Analysis — static and dynamic analysis of 
  malicious programs

**SOC monitors for:**
- System vulnerabilities
- Policy violations
- Unauthorized activity
- Network intrusions

**Incident Response phases:**
1. Preparation
2. Detection and Analysis
3. Containment, Eradication and Recovery
4. Post-Incident Activity

**Malware types learned:**
- Virus — spreads and damages files
- Trojan Horse — hides malicious function inside 
  legitimate-looking program
- Ransomware — encrypts files and demands payment

---

## Careers in Cyber
| Role | Focus |
|------|-------|
| Security Analyst | Monitor networks, compile reports, develop security plans |
| Security Engineer | Build and implement security solutions |
| Incident Responder | Respond to breaches, develop IR plans |
| Digital Forensics Examiner | Collect and analyse digital evidence |
| Malware Analyst | Reverse engineer and analyse malicious programs |
| Penetration Tester | Ethically hack systems to find vulnerabilities |
| Red Teamer | Simulate real attacks to test detection capabilities |

---

## How This Connects to My SOC Work
I currently work as a SOC analyst at TPSODL where I do 
exactly what this module describes — monitoring alerts 
in ArcSight SIEM, triaging incidents, validating IOCs, 
and coordinating incident response. The SIEM simulation 
in this module closely mirrors my day-to-day work of 
distinguishing true positives from false positives.

The Incident Response phases match our internal SOC 
runbooks — preparation, detection, containment and 
post-incident reporting are all part of our workflow.

---

## Key Takeaway
Offensive and defensive security are two sides of the 
same coin. Understanding how attackers think makes 
you a better defender — which is exactly why SOC 
analysts need both perspectives.

