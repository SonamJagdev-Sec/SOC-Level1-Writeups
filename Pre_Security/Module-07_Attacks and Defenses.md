# Module - 07
# Attacks and Defenses — TryHackMe Pre Security

## Rooms Covered
- The CIA Triad
- Cryptography Concepts
- Become a Hacker
- Become a Defender

---

## The CIA Triad

Cyber security is not just about stopping attacks — it is about
protecting three specific properties of digital information. These
three properties together form the **CIA Triad**, the foundational
mindset of every security professional.

**Confidentiality**
Ensures sensitive data is only accessible to authorized individuals.
- Breach example: Logging into social media on public Wi-Fi and having
  credentials intercepted by someone on the same network
- Protection methods: Encryption, access controls, VPNs

**Integrity**
Ensures data is not modified without authorization.
- Breach example: A bank transfer intercepted mid-flight and the
  destination account number changed before it completes
- Protection methods: Hashing, digital signatures, change auditing

**Availability**
Ensures data and services are accessible to authorized users when needed.
- Breach example: Attackers flood a website with requests until it
  crashes — no data was stolen, but the business still suffers
- Protection methods: Load balancing, redundancy, DDoS protection

**Quick Reference:**

| Scenario | CIA Pillar Affected |
|----------|-------------------|
| Credentials written on a sticky note | Confidentiality |
| Student attendance records modified after being locked | Integrity |
| Company website goes offline during business hours | Availability |
| Order price modified before checkout | Integrity |
| Personal document publicly available on the internet | Confidentiality |

**Key point:** CIA Triad is not just definitions — it is a security
mindset. Every incident can be assessed by asking: Was data exposed?
Was data modified? Was data unavailable?

---

## Cryptography Concepts

Cryptography is the practical mechanism that protects confidentiality
and integrity in the real world. When data travels across the internet,
it passes through dozens of routers and systems — cryptography ensures
only the intended recipient can read or trust it.

**Core Terms:**

| Term | Meaning |
|------|---------|
| Plaintext | Readable message — e.g., `HELLO` |
| Ciphertext | Scrambled output — e.g., `KHOOR` |
| Key | Secret value that controls encryption/decryption |
| Algorithm | The public method/recipe for using the key |

**Symmetric Encryption**

One key is used for both encrypting and decrypting.

- Example algorithm: **Caesar Cipher** (educational only — not secure)
  - Shifts each letter by a fixed number (the key)
  - Key of 3: `H→K`, `E→H`, `L→O`, `L→O`, `O→R` → `HELLO` = `KHOOR`
  - To decrypt: shift each letter backwards by the same key value

Encryption: plaintext + key + algorithm → ciphertext
Decryption: ciphertext + key + algorithm → plaintext

- Benefits: Very fast; efficient for large data volumes
- Weakness: **Key distribution problem** — how do you share the
  secret key safely before communication begins?

**Asymmetric Encryption**

Uses two mathematically linked keys — solves the key distribution problem.

- `Public Key` — shared openly; anyone can use it to encrypt a message
- `Private Key` — kept secret; only the owner can decrypt messages

**Mailbox Analogy:**
- The mail slot = public key (anyone can drop a letter in)
- The locked door = private key (only the owner can retrieve letters)

**How HTTPS uses both:**
1. Your browser requests the website's **public key** (wrapped in a certificate)
2. Asymmetric encryption is used to securely agree on a shared **symmetric key**
3. All further communication uses the faster **symmetric encryption**

This is the **hybrid approach** — asymmetric solves key distribution,
symmetric handles the bulk data.

**Certificate Authorities (CA):**
- A CA digitally signs a website's certificate to prove the public key
  is genuine
- Your browser checks: Is the CA trusted? Is the certificate valid?
- If either check fails → browser shows a security warning

**Symmetric vs Asymmetric — Side by Side:**

| Feature | Symmetric | Asymmetric |
|---------|-----------|------------|
| Keys used | One (same for both) | Two (public + private) |
| Speed | Fast | Slower |
| Key sharing | Must share secretly | Public key shared openly |
| Main use | Bulk data encryption | Key exchange, certificates |
| Analogy | One key locks and unlocks a box | Mailbox: anyone posts, only owner retrieves |

**Practical lab answers:**
- `CYBER` encrypted with Caesar key 5 = `HDGJW`
- Decoded message `FVZCYR PNRFNE PVCURE` (key 13) = `SIMPLE CAESAR CIPHER`

---

## Become a Hacker

Offensive security means thinking like an attacker — proactively
testing systems to find weaknesses before real attackers do. This is
also called **penetration testing** or **ethical hacking**.

**Key Terminology:**

| Term | Definition |
|------|-----------|
| Scope | Boundaries of what is allowed to be tested |
| Vulnerability | A weakness in a system that could be exploited |
| Exploit | A technique used to take advantage of a vulnerability |
| Enumeration | Collecting information about a system to find weak points |
| Credentials | Usernames and passwords used to authenticate |
| Dictionary Attack | Using a wordlist to systematically guess passwords |
| Red Teaming | Simulating a real adversary to test defenses end-to-end |

**Hacker Mindset:**
- Ask "What if it doesn't work as intended?"
- Test the unexpected — inputs developers didn't consider
- Chain small weaknesses — minor flaws combine into major impact
- Think like an adversary at every step

**Tools Used in the Lab:**

**Gobuster** — automated directory/page discovery
```bash
gobuster dir --url http://www.onlineshop.thm/ \
  -w /usr/share/wordlists/dirbuster/directory-list.txt
```
- Scans for hidden pages using a wordlist
- Status code `200` = page exists and is accessible

**Hydra** — automated password dictionary attack
```bash
hydra -l admin -P passlist.txt www.onlineshop.thm \
  http-post-form \
  "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```
- `-l admin` — set the username
- `-P passlist.txt` — wordlist of passwords to try
- `-V` — verbose output showing each attempt

**Lab Results — Chained Weakness Demonstration:**
1. Discovered hidden page `/login` (status `200`) via Gobuster
2. Tested common passwords manually against `admin` account
3. Valid credentials found: `admin` / `qwerty`
4. Post-login flag: `THM{born_to_hack!}`
5. Hydra made **17 failed attempts** before finding the correct password

**Key lesson:** A hidden login page alone seems harmless. Paired with
a weak password, it becomes a full account takeover. This is the
domino effect of chained weaknesses.

---

## Become a Defender

Defensive security (Blue Team) focuses on understanding what needs
protecting, gaining visibility, and applying layered controls to
prevent, detect, and respond to attacks.

**City Analogy for Client Infrastructure:**

| Infrastructure Component | City Equivalent | Purpose |
|--------------------------|----------------|---------|
| Employee Devices | Homes | Where users work |
| Web Server | Shops / Public buildings | Hosts websites and apps |
| Mail Server | Post office | Sends and receives email |
| Firewall | City gate | Controls inbound/outbound traffic |
| Internet | Everything outside the walls | Untrusted external networks |

**Five Defender Responsibilities:**
- `Prevention` — controls in place before an attack (firewalls, patching)
- `Detection` — monitoring for suspicious activity (logs, SIEM alerts)
- `Mitigation` — limiting damage during an incident (isolating systems)
- `Analysis` — investigating what happened and what was impacted
- `Response and Improvement` — recovering and hardening defenses

**Defender Mindset Principles:**
- **Threat Anticipation** — ask "What if?" for every system
- **Attack Awareness** — study common attack chains and frameworks
- **Risk Prioritization** — focus effort on high-value assets first
- **Continuous Adaptation** — threats evolve; defense must evolve too

**Available Defenses by System:**

| System | Threat | Defense |
|--------|--------|---------|
| Employee Devices | Malware via phishing | Antivirus, software updates |
| Web Server | Web application attacks | WAF, HTTPS, secure config |
| Mail Server | Phishing, malicious attachments | Spam filters, attachment scanning |
| Firewall | Unauthorized inbound access | Firewall rules, IP blocking |

**Lab Results:**
- Infrastructure mapping flag: `THM{mapping_infrastructure!}`
- Defending city infrastructure flag: `THM{defensive_techniques!}`

---

## How This Connects to My SOC Work

This module is the most directly relevant to my current role at TPSODL.

**CIA Triad** is the framework I use every day without always naming
it explicitly. When I triage an ArcSight alert, my first questions are:
Was data accessed by an unauthorized user (Confidentiality)? Was data
altered without approval (Integrity)? Is a critical service unavailable
(Availability)? Every alert maps to one of these three pillars.

**Cryptography** is essential background knowledge for understanding
HTTPS inspection in our network, SSL certificate alerts, and why
certain detection rules fire on unencrypted traffic. When I see an
alert for cleartext credential submission or an expired certificate, I
now fully understand what the underlying risk is and why it matters.

**Become a Hacker** directly mirrors the threat scenarios I defend
against. The Gobuster scan replicates web enumeration attacks I've
seen in alerts — directory brute force attempts show up as high-volume
404 errors in web server logs. The Hydra dictionary attack maps
directly to brute-force login alerts that trigger in ArcSight when
there are multiple failed authentication attempts followed by a
success. Understanding exactly how these tools work makes me a
significantly better analyst when triaging such events.

**Become a Defender** reflects our SOC team's day-to-day structure.
The five defender responsibilities — Prevention, Detection, Mitigation,
Analysis, and Response — directly mirror the incident lifecycle we
follow in our runbooks. The city analogy is a useful mental model I
can use when explaining our security architecture to non-technical
stakeholders.

---

## Key Takeaway

Module 07 completes the Pre Security path by connecting all previous
knowledge into the two sides of cyber security. Attackers and
defenders both need to understand the same systems — the difference
is intent and permission. A SOC analyst who thinks like an attacker
is a far better defender, because they know what the attacker is
looking for, what tools they use, and where the weak points are in
the chain.
