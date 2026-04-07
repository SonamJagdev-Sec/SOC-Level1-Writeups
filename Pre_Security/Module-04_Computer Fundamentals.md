# Module-04
# Computer Fundamentals — TryHackMe Pre Security

## Rooms Covered
- Inside a Computer System
- Computer Types
- Client-Server Basics
- Virtualisation Basics
- Cloud Computing Fundamentals

---

## Inside a Computer System

**Analogy: The Castle**
Before protecting a castle, we need to know its layout — where the treasure room,
food storage, and commander's quarters are.
> Trying to defend what you don't understand is like defending a castle you have never seen.

**Core Components of a Computer:**
Every computer system includes the same building blocks — each part has its own job,
and together they make the computer work.

| Component | Role | Human Body Analogy |
|-----------|------|--------------------|
| CPU | Processes all instructions | Brain |
| RAM | Temporary fast-access memory | Short-term memory |
| Storage (HDD/SSD) | Permanent data storage | Long-term memory |
| PSU | Powers all components | Heart |
| Motherboard | Connects all components together | Nervous system |



---## How This Connects to MY SOC Work

Understanding computer fundamentals is not just theory —
every concept in this module directly applies to real SOC analyst work.
- Knowing CPU, RAM, and storage helps you understand
  what malware does when it runs on a system
- High CPU or RAM usage on an endpoint can be an
  indicator of a cryptominer or ransomware running in the background
- SOC analysts review process and memory activity during
  incident response to find malicious behaviour

## What Happens When You Press the Start Button?

**The Boot Process (Step by Step):**

1. **Press Power Button** — Signal sent to PSU to allow power to flow
2. **Firmware Starts** — UEFI/BIOS initialises all core components
3. **Power-On Self Test (POST)** — Tests if every required component is present and functioning correctly
4. **Select Boot Device** — UEFI checks its ordered list to find where the OS bootloader is located
5. **Initiate Bootloader** — Bootloader transfers the OS from storage into RAM, then UEFI hands control to the OS

**Key Terms:**
- **UEFI** — Unified Extensible Firmware Interface — modern firmware that manages component startup
- **BIOS** — older version of UEFI, mostly replaced now
- **POST** — Power-On Self Test — checks all components before boot
- **Bootloader** — loads the Operating System into RAM

> ⚠️ The boot process is a target for hackers — malware can embed itself here to persist across reboots

## How This Connects to SOC Work
- Attackers use **bootkits** and **rootkits** to embed malware
  into the boot process so it survives reboots and reinstalls
- As a SOC analyst you need to recognise when a system's
  boot sequence has been tampered with
- Tools like Secure Boot and UEFI integrity checks are
  defence mechanisms you will encounter in enterprise environments
---

## Computer Types

**Computers You Sit In Front Of:**

| Computer Type | Screen & Keyboard | Main Purpose |
|---------------|-------------------|--------------|
| Laptop | Yes | Portable everyday computing |
| Desktop | Yes | Sustained performance at a fixed location |
| Workstation | Yes | Precision and reliability for professional tasks |
| Server | No | Providing services to many users over a network |

**Hidden Computers (You Don't Sit In Front Of):**

| Type | What It Is | Examples |
|------|-----------|---------|
| Smartphone | Pocket-sized computer optimised for battery and connectivity | iPhone, Android |
| Tablet | Touch-first computer with larger screen | iPad, drawing tablet |
| IoT Device | Network-connected device with a single purpose | Thermostat, smart doorbell, fitness tracker |
| Embedded Computer | Computer built inside another device | Coffee maker controller, automatic door sensor |

**IoT vs Embedded — Key Difference:**
- **IoT** — connects to a network to send/receive data
- **Embedded** — works inside a machine, may not connect to anything at all

**Why Computers Come in Different Designs:**

Every design is a trade-off:
- **Mobility costs performance** — portable devices sacrifice sustained power to stay small and cool
- **Reliability costs money** — servers use redundancy (extra power supplies, disks) to avoid failure
- **Purpose shapes everything** — there is no best computer, only the right tool for the job

## How This Connects to SOC Work
- SOC analysts monitor many different device types —
  not just laptops and desktops but also servers, IoT devices,
  and embedded systems
- IoT devices are often unpatched and poorly secured,
  making them a common entry point for attackers
- Embedded systems in industrial environments (OT/ICS)
  are high-value targets and require specialised monitoring
---

## Client-Server Basics

**History:**
Initially, computers worked alone — stored their own files and ran their own programs.
Networks like **ARPANET, CYCLADES, NPL, and NSFNET** paved the way for the modern internet,
allowing systems to interconnect and specialise.

**Pizza Delivery Analogy:**

| Pizza Analogy | Computer Equivalent |
|---------------|-------------------|
| Alice (customer) | Client — initiates the request (e.g. browser) |
| Luigi's Pizza | Server — listens and responds to requests |
| Bob driving | Protocol — defines how client and server communicate |
| Pizza menu | Available commands / resources |
| Door number | Port — identifies a specific service on the server |
| GPS / Luigi's address | DNS — translates domain name to IP address |

**Key Concepts:**

- **Client** — always initiates the request
- **Server** — responds to client requests
- **Protocol** — defines rules, syntax, and commands for communication
- **Port** — a numbered entry point that identifies a specific service
- **DNS** — Domain Name Service — resolves domain names to IP addresses
- **IP Address** — the location address of a server on a network

## How This Connects to SOC Work
- Almost every attack you will investigate involves
  a client making requests to a server
- Understanding who initiated a connection (client)
  and who responded (server) is fundamental to
  reading network logs and SIEM alerts
- Command and Control (C2) malware uses the client-server
  model — infected machines (clients) reach out to attacker
  servers to receive instructions
---

## HTTP in Detail

**HTTP(S) = HyperText Transfer Protocol (Secure)**
- Stateless client-server protocol used for the World Wide Web
- Each request is processed independently — server does not remember previous requests
- Modern websites use **cookies / session tokens** to introduce statefulness at the application level

**HTTP Methods (9 Core):**

| Method | Purpose |
|--------|---------|
| GET | Retrieve a resource from the server |
| POST | Submit data to the server |
| PUT | Update existing data |
| DELETE | Remove data |
| PATCH | Partially update data |
| HEAD | Same as GET but returns headers only |
| OPTIONS | Returns supported methods for a resource |
| CONNECT | Establishes a tunnel to the server |
| TRACE | Diagnostic — echoes the request back |

**GET Request — Key Fields:**

| Field | Description |
|-------|-------------|
| Scheme | Protocol used — HTTP or HTTPS |
| Host | Name of the server being requested |
| Filename | File or resource requested (e.g. `/` = `index.html`) |
| Address | IP address of the server |
| Status | Whether request succeeded (e.g. `200 OK`) |

**Request & Response Structure:**
- **Response Header** — metadata about the response
- **Response Body** — the actual requested content (HTML, JSON etc.)

## How This Connects to SOC Work
- Web-based attacks (phishing, SQLi, XSS) all happen over HTTP/HTTPS
- SOC analysts examine HTTP request logs to spot
  suspicious methods, unusual paths, or unexpected status codes
- Knowing common ports helps identify anomalies —
  e.g. traffic on port 4444 is a red flag (common Metasploit port)
- Unexpected open ports on a server can indicate
  a backdoor or misconfiguration

---

## Virtualisation Basics

**The Problem Before Virtualisation:**
The old rule of thumb in IT was: **"One server = one application"**

Problems with this approach:
- **High cost** — hardware, electricity, cooling, maintenance, data center space
- **Low utilisation** — most servers ran at only 5–20% capacity
- **Slow deployment** — setting up physical servers took days or weeks
- **Hard to scale** — more demand meant buying yet another server

**The Building Analogy:**

| Building Analogy | Virtualisation Equivalent |
|-----------------|--------------------------|
| The building | Physical server |
| Apartments | Virtual Machines (VMs) |
| Tenants | Applications / Operating Systems |
| Building manager | Hypervisor |


---

## Hypervisor (The Building Manager)

A **hypervisor** is the software that creates and manages virtual machines.

It:
- Divides a physical computer into multiple virtual ones
- Gives each VM its own share of CPU, memory, and storage
- Keeps everything isolated and safe
- Manages VM lifecycle — start, stop, pause, clone, delete

**Hypervisor Types:**

| Type | Runs On | Best For |
|------|---------|---------|
| Type 1 | Directly on physical hardware | Servers, data centers, production environments |
| Type 2 | Inside an existing OS | Learning, testing, home labs |

**Use Case Reference:**

| Use Case | Type 1 | Type 2 |
|----------|--------|--------|
| Test Malicious Files | | ✅ |
| Production Server | ✅ | |
| Database Server | ✅ | |
| Software Testing | | ✅ |
| Kali Linux | | ✅ |
| Data Center | ✅ | |

> ⚠️ When testing malware in a VM, isolate the guest from the host to prevent infection

---

## Virtual Machines (The Apartments)

A **Virtual Machine (VM)** is a virtual computer created by the hypervisor.

- Has its own virtual CPU, RAM, storage, and network
- Can run any OS — Windows, Linux, macOS etc.
- Completely isolated from other VMs — if one breaks, others keep running

**Common VM Tools (Type 2):**
- Oracle VirtualBox
- VMware Workstation

**Common Use Cases:**
- Running Kali Linux without buying a second machine
- Safely testing potentially malicious files in an isolated environment

---

## Containers (The Rooms Inside the Apartment)

A **container** is a lightweight, isolated environment that runs a single application
and all its dependencies — without needing a full OS.

- Shares the **host's kernel** instead of running its own OS
- Starts almost instantly and uses fewer resources than a full VM
- Must match the host system type (e.g. can't run a Windows container on Linux)

**Container characteristics:**
- Packages the app and its dependencies (libraries, tools, versions)
- Isolated from each other — a misbehaving container doesn't affect others
- Runs consistently on any machine — perfect for development and scalable deployments

**Docker** — the most popular platform for building, deploying, and running containers

**VM vs Container Summary:**

| | Virtual Machine | Container |
|-|----------------|-----------|
| Size | Full OS included | Shares host OS kernel |
| Startup | Slower | Almost instant |
| Isolation | Complete | Isolated but shares kernel |
| Best for | Full flexibility, max isolation | Lightweight, scalable apps |

**Key Virtualisation Terminology:**

| Term | Definition |
|------|-----------|
| Virtualisation | Enables one physical computer to act as multiple separate computers |
| Hypervisor | Software that creates and manages VMs |
| Virtual Machine (VM) | A full virtual computer inside a physical one |
| Container | A small isolated box for one app, sharing the host OS |
| Container Image | A pre-packed template used to create containers |
| Network Port | Numbered entry points apps use to communicate over the network |

**Key Benefits of Virtualisation:**
- Cost savings
- Better resource utilisation
- Safe testing for cyber security
- Faster deployment
- Flexibility, portability, scalability
- Centralised management

## How This Connects to SOC Work
- Most enterprise infrastructure runs on VMs —
  understanding this helps you investigate alerts in virtualised environments
- Malware analysis is done inside isolated VMs to
  safely detonate and study malicious files
- Attackers attempt **VM escape** techniques to break out
  of sandboxes — knowing how VMs work helps you understand this threat
- Containers (Docker) are increasingly targeted —
  misconfigured containers can give attackers access to the host system
---

## Cloud Computing Fundamentals

**What is Cloud Computing?**
Cloud computing provides computing resources — servers, storage, networking — over the internet,
enabling applications to be flexible, scalable, and cost-effective without managing physical hardware.

**How Servers Evolved to Cloud:**
Physical Servers → Virtualisation → Containers → Cloud Computing

**Cloud Benefits & Characteristics:**

| Benefit | Description |
|---------|-------------|
| Scalability | Scale up or down as needs change |
| On-demand self-service | Create or remove servers instantly |
| Pay only for what you use | Charged based on usage, not upfront costs |
| Security | Cloud providers protect the infrastructure |
| High Availability | App keeps running even if part of the system fails |
| Global Access | Accessible by users anywhere in the world |

---

## Cloud Deployment Types

| Type | Description | Best For |
|------|-------------|---------|
| Public Cloud | Shared infrastructure over the internet | Startups, websites, global apps |
| Private Cloud | Dedicated to one organisation | Banks, healthcare, government |
| Hybrid Cloud | Mix of public and private clouds | E-commerce — sensitive data private, public scaling during peak |

---

## Cloud Service Models

| Model | Who Manages What | Example |
|-------|-----------------|---------|
| IaaS (Infrastructure as a Service) | Provider manages hardware — you manage OS and app | AWS EC2, Azure VMs |
| PaaS (Platform as a Service) | Provider manages hardware and OS — you manage app only | Google App Engine |
| SaaS (Software as a Service) | Provider manages everything — you just use the app | Gmail, Zoom |

**Analogy — Renting a Place to Live:**
- **IaaS** — renting an empty flat (you furnish and manage it)
- **PaaS** — renting a furnished flat (you just bring your stuff)
- **SaaS** — staying in a hotel (everything is managed for you)

---

## Major Cloud Vendors

| Vendor | Known For |
|--------|----------|
| AWS (Amazon Web Services) | Industry leader — most extensive offerings and global reach |
| Microsoft Azure | Enterprise and hybrid cloud environments |
| Google Cloud Platform (GCP) | Data analytics, AI, and machine learning |
| Alibaba Cloud | Major player in Asia |
| IBM Cloud | Hybrid cloud and AI-driven solutions |
| Oracle Cloud | Enterprise applications and databases |

**AWS Key Terminology:**

| Term | Description |
|------|-------------|
| EC2 | Virtual computer (server) in the cloud |
| Instance Type | Defines how powerful the VM is (e.g. t3.micro, m5.large) |
| Region | Geographical location where resources are hosted |

**Real-World Cloud Usage:**
- **Netflix** — runs entire platform on AWS, scales globally for millions of users
- **Spotify** — handles millions of songs and users, scales quickly for new releases
- **Instagram** — stores massive amounts of photos/videos, delivers globally
- **Online stores** — handle traffic spikes during Black Friday without permanent infrastructure

---

## Key Takeaway
Cloud computing is built on virtualisation and containerisation.
Understanding these layers — from physical hardware → VMs → containers → cloud — is essential
for any SOC analyst working with modern infrastructure, incident response, or threat detection.

How this connects to my soc work 
- Modern organisations run workloads on AWS, Azure, or GCP —
  SOC analysts must understand cloud infrastructure to investigate alerts
- Cloud misconfigurations are one of the biggest causes
  of data breaches (e.g. publicly exposed S3 buckets)
- Cloud environments generate their own logs (CloudTrail, Azure Monitor)
  which SOC analysts use to detect suspicious activity
- Understanding IaaS, PaaS, and SaaS helps you know
  what your organisation is responsible for securing vs what the provider handles

