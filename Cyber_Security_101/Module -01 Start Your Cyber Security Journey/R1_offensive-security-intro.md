# Room 01: Offensive Security Intro
# TryHackMe — Cyber Security 101


## What is Offensive Security?

Offensive security is about thinking like an attacker to find and fix vulnerabilities **before** real hackers do. The goal is not to cause harm but to expose weaknesses so they can be patched.

> *"To outsmart a hacker, you need to think like one."*

Professionals who do this are called **Penetration Testers** or **Ethical Hackers**. They are given legal permission to attack systems and report findings to the organization.

---

## Key Tool Practiced: `dirb`

`dirb` is a brute-force web content scanner. It works by taking a wordlist of common page/directory names and testing them one by one against a target URL to find hidden or exposed pages.

**Command used:**
```bash
dirb http://fakebank.thm
```

**What it does:**
- Takes the default wordlist at `/usr/share/dirb/wordlists/common.txt`
- Sends HTTP requests for each word in the list
- Reports back URLs that returned a valid response (HTTP 200, 301, etc.)

**Output from the lab:**
```
+ http://fakebank.thm/bank-deposit (CODE:200|SIZE:4663)
+ http://fakebank.thm/images (CODE:301|SIZE:179)
```

The hidden page `/bank-deposit` was not linked anywhere on the website — but `dirb` found it by brute-forcing common names.

---

## Lab Walkthrough: Hacking FakeBank

The lab simulated hacking a fake banking application called **FakeBank**.

**Steps performed:**

1. Opened a terminal on the AttackBox
2. Ran `dirb http://fakebank.thm` to find hidden URLs
3. Discovered the hidden page: `http://fakebank.thm/bank-deposit`
4. Navigated to the page in the browser
5. Used the deposit form to add $2000 to bank account **#8881**
6. Returned to the account page to confirm the new balance

This demonstrated how an attacker can find and exploit hidden functionality that was never meant to be publicly accessible.

---

## Key Concepts

- **Hidden URLs** — Web applications sometimes expose sensitive functionality through URLs that are not linked from the main site but still accessible if you know the path
- **Brute Force** — Trying many possibilities (wordlist entries) systematically to discover resources
- **Wordlists** — Predefined lists of common file/directory names used in security testing
- **HTTP Status Codes** — `200` means the page exists and is accessible; `301` means a redirect

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| Which option better represents simulating a hacker's actions to find vulnerabilities? | Offensive Security |
| What is your bank account number in the FakeBank web application? | 8881 |
| What is the hidden URL found by dirb (other than /images)? | /bank-deposit |
| What is the flag shown after successfully adding funds? | *(obtained during lab)* |

---

## How This Connects to My SOC Work

As a SOC analyst at TPSODL, understanding offensive techniques is critical to my defensive work. When I triage alerts in **ArcSight SIEM**, I need to recognize what attackers are doing — and `dirb`-style directory brute-forcing is something I'd see in web application attack alerts (e.g., a spike in 404s followed by a 200 on a sensitive URL).

Knowing how `dirb` works helps me:
- Identify web content discovery attempts in logs
- Understand what an attacker is looking for
- Write better detection rules for this type of activity

---

## Key Takeaway

Offensive security is not just for red teamers. Every SOC analyst and defender benefits from understanding how attacks work at a technical level. You cannot detect what you don't understand.
