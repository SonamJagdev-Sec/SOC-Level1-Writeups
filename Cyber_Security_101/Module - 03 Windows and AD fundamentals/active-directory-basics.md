# Room 04: Active Directory Basics
# TryHackMe — Cyber Security 101 | Module 03: Windows and AD Fundamentals



## Overview

Active Directory (AD) is the backbone of almost every corporate Windows environment. It centralizes identity management, access control, and security policy enforcement across an entire organization. As a SOC analyst, understanding AD is non-negotiable — virtually every enterprise attack chain involves AD in some way.

---

## What is a Windows Domain?

A **Windows Domain** is a group of users and computers under the administration of a business, managed from a central repository called **Active Directory (AD)**.

The server that runs AD services is called the **Domain Controller (DC)**.

**Why use a domain instead of standalone machines?**

| Problem (No Domain) | Solution (With Domain) |
|--------------------|----------------------|
| Manual config on each machine | Centralized management from one place |
| Separate credentials per machine | Single set of credentials works everywhere |
| No policy enforcement | GPOs enforce policies across all machines |
| On-site support for every issue | Remote administration via DC |

**Credentials are stored in:** Active Directory  
**Server running AD services:** Domain Controller

---

## Active Directory — Core Components

### Users (Security Principals)

Users are the most common AD object. They represent:
- **People** — Employees who need network access
- **Services** — Accounts used by services like IIS or MSSQL (least-privilege accounts)

### Machines (Security Principals)

Every computer that joins the AD domain gets a **machine account** created automatically.

- Machine accounts are local admins on their own computer
- Named with a `$` suffix: a machine named `TOM-PC` has account `TOM-PC$`
- Passwords are auto-rotated (120 random characters)

### Security Groups

Groups allow assigning permissions to multiple users at once instead of individually.

**Default important groups:**

| Group | Description |
|-------|-------------|
| Domain Admins | Full administrative control over the entire domain |
| Server Operators | Can administer DCs, cannot change admin group memberships |
| Backup Operators | Can access any file regardless of permissions — for backups |
| Account Operators | Can create/modify non-admin accounts |
| Domain Users | All user accounts in the domain |
| Domain Computers | All computers joined to the domain |
| Domain Controllers | All DCs in the domain |
| Enterprise Admins | Admin privileges across ALL domains in the forest |

> **Group that normally administers all computers and resources in a domain:** Domain Admins

---

## Organizational Units (OUs)

OUs are **container objects** used to organize users, computers, and groups within AD. They are the primary way to apply Group Policy to specific sets of users or machines.

**Key rules:**
- A user can only belong to **one OU** at a time
- OUs typically mirror the business structure (e.g., Sales, IT, Management)
- Policies are applied per OU — different departments get different configurations

**Default containers in AD:**

| Container | Contents |
|-----------|---------|
| Builtin | Default groups for any Windows host |
| Computers | Machines joined to the domain (default landing zone) |
| Domain Controllers | All DCs in the domain |
| Users | Default users and domain-wide groups |
| Managed Service Accounts | Service accounts |

### Security Groups vs OUs

| Feature | Security Groups | OUs |
|---------|----------------|-----|
| Purpose | Grant permissions to resources | Apply policies to users/computers |
| User membership | Can be in many groups | Can only be in one OU |
| Example use | Allow access to a shared folder | Deploy a password policy to IT dept |

> **For Quality Assurance department users — use:** Organizational Units (OUs)

---

## Managing Users in AD

### Deleting OUs

By default, OUs are **protected against accidental deletion**.

To delete an OU:
1. Enable **Advanced Features** in the View menu
2. Right-click the OU → Properties → Object tab
3. Uncheck **"Protect object from accidental deletion"**
4. Then delete the OU

### Delegation

**Delegation** allows granting specific users control over certain OUs without making them Domain Admins.

**Common use case:** Grant IT support the ability to reset passwords for Sales/Marketing/Management users.

**Lab example — Delegating password resets to Phillip:**
1. Right-click the Sales OU → Delegate Control
2. Add user `phillip` → Check Names
3. Select "Reset user passwords and force password change at next logon"

**PowerShell commands used:**
```powershell
# Reset a user's password
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose

# Force password change at next logon
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

> **The process of granting privileges to a user over an OU:** Delegation

---

## Managing Computers in AD

Best practice — segregate machines into three categories:

| Category | Description |
|----------|-------------|
| Workstations | Daily-use machines for regular users; no privileged accounts |
| Servers | Provide services to users or other servers |
| Domain Controllers | Manage AD; most sensitive devices — contain all password hashes |

> **Lab:** After organizing computers, workstations OU and servers OU were populated separately. It is **recommended (yay)** to create separate OUs for Servers and Workstations.

---

## Group Policy Objects (GPOs)

**GPOs** are collections of settings that can be applied to OUs to enforce configurations and security baselines across users and computers.

**Tool:** Group Policy Management (`gpmc.msc`)

**How GPOs work:**
1. Create a GPO under Group Policy Objects
2. Link it to the target OU
3. The GPO applies to that OU and all child OUs

**GPO distribution:** GPOs are distributed via the **SYSVOL** network share, stored on the DC at `C:\Windows\SYSVOL\sysvol\`. All domain users sync GPOs from this share.

**Force immediate GPO sync:**
```powershell
gpupdate /force
```

### Lab GPOs Created

**1. Restrict Control Panel Access**
- Blocks non-IT users from accessing Control Panel
- Location: `User Configuration → Administrative Templates → Control Panel`
- Linked to: Marketing, Management, Sales OUs

**2. Auto Lock Screen**
- Locks workstations after 5 minutes of inactivity
- Location: `Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options → Interactive logon: Machine inactivity limit`
- Linked to: Root domain (inherited by all child OUs)

> **Network share used to distribute GPOs:** SYSVOL  
> **Can a GPO apply to both users and computers?** Yes (yay)

---

## Authentication Methods

Two protocols handle network authentication in Windows domains:

### Kerberos (Default — Modern Windows)

Kerberos uses a **ticket-based** system. Users prove they've already authenticated by presenting tickets rather than credentials repeatedly.

**Authentication flow:**

```
1. User → KDC: Username + encrypted timestamp
         ↓
2. KDC → User: TGT (Ticket Granting Ticket) + Session Key
         ↓
3. User → KDC: TGT + SPN (service name) → requests TGS
         ↓
4. KDC → User: TGS (Ticket Granting Service) + Service Session Key
         ↓
5. User → Service: TGS to authenticate
         ↓
6. Service decrypts TGS using its own password hash → grants access
```

**Key components:**
- **KDC (Key Distribution Center)** — Runs on the DC; issues tickets
- **TGT (Ticket Granting Ticket)** — Allows requesting service tickets; encrypted with `krbtgt` hash
- **TGS (Ticket Granting Service)** — Grants access to a specific service
- **SPN (Service Principal Name)** — Identifies the service you want to access

> **Ticket that allows requesting further TGS tickets:** Ticket Granting Ticket (TGT)

### NetNTLM (Legacy — Challenge-Response)

```
1. Client → Server: Authentication request
2. Server → Client: Random challenge number
3. Client → Server: Response (NTLM hash + challenge combined)
4. Server → DC: Forwards challenge + response for verification
5. DC → Server: Authentication result
6. Server → Client: Access granted or denied
```

**Key point:** The user's password/hash is **never transmitted** over the network.

> **Will modern Windows use NetNTLM as preferred protocol by default?** No (nay)  
> **Is a user's password transmitted over the network in NetNTLM?** No (nay)

---

## Trees, Forests & Trusts

As organizations grow, a single domain may not be sufficient.

### Trees

Multiple domains sharing the **same namespace** joined together.

**Example:**
```
thm.local (root)
├── uk.thm.local
└── us.thm.local
```

Each subdomain has its own DC and can be managed independently. The **Enterprise Admins** group has admin rights across all domains in the tree.

### Forests

Multiple domain **trees** with **different namespaces** combined into one network.

**Example:**
```
thm.local (Tree 1)       mht.local (Tree 2)
├── uk.thm.local         ├── asia.mht.local
└── us.thm.local         └── eu.mht.local
```

> **A group of Windows domains sharing the same namespace:** Tree

### Trust Relationships

Trusts allow users from one domain to access resources in another domain.

| Trust Type | Direction | Access |
|-----------|-----------|--------|
| One-way trust | AAA trusts BBB | Users in BBB can access resources in AAA |
| Two-way trust | Both trust each other | Users in both domains can access each other's resources |

> Joining domains under a tree/forest creates **two-way trust** by default.  
> Trust ≠ automatic access — you still control **what** is authorized.

> **What must be configured between two domains for cross-domain resource access:** A Trust Relationship

---

## Questions & Answers

| Question | Answer |
|----------|--------|
| Credentials stored in a Windows domain? | Active Directory |
| Server running AD services? | Domain Controller |
| Group that administers all computers and resources? | Domain Admins |
| Machine account name for TOM-PC? | TOM-PC$ |
| Container type for QA department users? | Organizational Units |
| Process of granting privileges over an OU? | Delegation |
| Workstations OU count after organizing? | 7 |
| Is it recommended to separate Servers and Workstations OUs? | yay |
| Network share used to distribute GPOs? | SYSVOL |
| Can a GPO apply settings to users and computers? | yay |
| Will modern Windows use NetNTLM as default? | nay |
| Ticket that allows requesting TGS? | Ticket Granting Ticket |
| Is user password transmitted in NetNTLM? | nay |
| Group of domains sharing the same namespace? | Tree |
| What must be configured for cross-domain access? | A Trust Relationship |

---

## How This Connects to My SOC Work

Active Directory is at the center of almost every enterprise security investigation I handle at TPSODL:

- **Domain Admins / Privilege Escalation** — Many alerts I triage in ArcSight involve accounts suddenly gaining Domain Admin privileges or attempting to add themselves to privileged groups. Understanding the AD group structure helps me immediately assess the impact.
- **Kerberos attacks** — Kerberoasting, Pass-the-Ticket, Golden Ticket, and Silver Ticket attacks all target Kerberos. Knowing how TGTs and TGSs work is essential for recognizing these attack patterns in logs (e.g., Event ID 4769 — Kerberos Service Ticket Request).
- **NetNTLM / NTLM relay attacks** — Still very much alive in corporate environments. NTLM relay, Pass-the-Hash, and credential capture attacks all exploit the challenge-response flow covered here.
- **GPOs as an attack vector** — Attackers with sufficient privileges can modify GPOs to deploy malware or create backdoors across all domain-joined machines simultaneously. Detecting unauthorized GPO changes is a critical detection use case.
- **Delegation abuse** — Misconfigured delegation (especially unconstrained delegation) is a well-known AD attack path. Understanding legitimate delegation helps me spot when it's being abused.
- **SYSVOL** — Attackers often search SYSVOL for scripts containing hardcoded credentials. Monitoring access to SYSVOL from non-DC machines is a useful detection signal.
- **Trees & Forests / Trust Abuse** — In environments with multiple domains, trust relationships can be exploited for lateral movement across domain boundaries. Understanding the trust model helps me scope the blast radius of a compromise.

---

## Key Takeaway

Active Directory is simultaneously the most powerful tool for managing a corporate network and the most targeted component of it. Every major enterprise attack — from ransomware to APT intrusions — involves AD in some way, whether for initial access, lateral movement, or persistence. Understanding AD at a foundational level is not optional for a SOC analyst — it is the baseline.
