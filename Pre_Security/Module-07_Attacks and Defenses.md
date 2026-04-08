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