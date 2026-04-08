# Module-02
# Network Fundamentals — TryHackMe Pre Security

## Rooms Covered
- What is Networking?
- Intro to LAN
- OSI Model
- Packets & Frames
- Extending Your Network

---

## What is Networking?

**Key concepts:**
- A network is two or more devices connected together
- The Internet is one giant network made of many small networks
- The Internet was invented by Tim Berners-Lee in 1989 
  via the World Wide Web (WWW)
- First iteration of the Internet was ARPANET (1960s) 
  funded by the US Defence Department

**Device Identification — two ways:**
- IP Address (logical — can change)
- MAC Address (physical — permanent, like a fingerprint)

**IP Address types:**
- Private IP — identifies device within local network
  (e.g. 192.168.1.77)
- Public IP — identifies device on the Internet
  (assigned by ISP)

**IPv4 vs IPv6:**
- IPv4 — supports 2^32 addresses (4.29 billion) — running out
- IPv6 — supports 2^128 addresses (340 trillion+) — solution

**MAC Address:**
- 12-character hexadecimal number 
  (e.g. a4:c3:f0:85:ac:2d)
- First 6 characters = manufacturer
- Last 6 characters = unique device ID
- Can be spoofed — security risk

**Ping:**
- Uses ICMP (Internet Control Message Protocol)
- Tests connectivity between devices
- Syntax: `ping <IP address or URL>`

---

## Intro to LAN

**LAN = Local Area Network**

**Network Topologies:**

| Topology | Advantage | Disadvantage |
|----------|-----------|--------------|
| Star | Reliable, scalable | Expensive, depends on central device |
| Bus | Cheap, easy to set up | Single point of failure, bottleneck |
| Ring | Easy to troubleshoot | One break = entire network fails |

**Key Devices:**
- **Switch** — connects multiple devices, sends data 
  only to intended device (Layer 2/3)
- **Router** — connects different networks, routes 
  data between them (Layer 3)
**Subnetting:**
- Dividing a network into smaller sub-networks
- Subnet mask = 32 bits, range 0-255 per octet
- Three address types:
  - Network Address — identifies start of network
  - Host Address — identifies individual devices
  - Default Gateway — sends data to other networks
    
**ARP (Address Resolution Protocol):**
- Maps IP addresses to MAC addresses
- Sends ARP Request (broadcast) → receives ARP Reply
- Stores results in ARP cache

**DHCP (Dynamic Host Configuration Protocol):**
- Automatically assigns IP addresses to devices
- Process: Discover → Offer → Request → ACK

---

## OSI Model

**OSI = Open Systems Interconnection**
**7 layers — data travels through each with encapsulation**

| Layer | Name | Key Responsibility |
|-------|------|--------------------|
| 7 | Application | User interaction, DNS, HTTP |
| 6 | Presentation | Data formatting, encryption (HTTPS) |
| 5 | Session | Creates/manages/closes sessions |
| 4 | Transport | TCP/UDP, error checking, ports |
| 3 | Network | IP addressing, routing (OSPF, RIP) |
| 2 | Data Link | MAC addressing, frames |
| 1 | Physical | Electrical signals, binary, cables |


**TCP vs UDP (Layer 4):**

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-based | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Speed | Slower | Faster |
| Use case | Web, email, file transfer | Streaming, gaming, VoIP |

**TCP Three-Way Handshake:**
1. SYN — client initiates connection
2. SYN/ACK — server acknowledges
3. ACK — client confirms, data transfer begins
4. FIN — connection closed cleanly
5. RST — connection abruptly terminated

---

## Packets & Frames

- **Packet** — data at Layer 3 (has IP address info)
- **Frame** — data at Layer 2 (has MAC address info)
- Encapsulation = adding headers at each layer
- Decapsulation = stripping headers at destination

**Key TCP Packet Headers:**
- Source/Destination IP
- Source/Destination Port
- Sequence Number
- Acknowledgement Number
- Checksum — ensures data integrity
- TTL (Time to Live) — prevents packets looping forever
- Flags — SYN, ACK, FIN, RST


**Common Ports to Remember:**

| Protocol | Port |
|----------|------|
| FTP | 21 |
| SSH | 22 |
| HTTP | 80 |
| HTTPS | 443 |
| SMB | 445 |
| RDP | 3389 |

## Extending Your Network

**Firewalls:**
- Controls what traffic enters/exits a network
- Operates at Layers 3 & 4 of OSI model
- Two types:
  - **Stateful** — inspects entire connection, 
    more resource-intensive, smarter
  - **Stateless** — inspects individual packets, 
    faster, uses static rules

**VPN (Virtual Private Network):**
- Creates encrypted tunnel between networks
- Benefits: connects remote offices, privacy, anonymity
- VPN Technologies:
  - PPP — authentication and encryption
  - PPTP — easy to set up, weakly encrypted
  - IPSec — strong encryption, harder to set up

**Port Forwarding:**
- Allows external access to internal services
- Configured at the router
- Example: exposing internal web server (port 80) 
  to the Internet

---

## How This Connects to My SOC Work

This entire section directly maps to my daily SOC work 
at TPSODL:

- I analyze HTTP/HTTPS, DNS and proxy logs in ArcSight 
  SIEM — understanding packets and ports is essential
- I monitor for MAC spoofing and ARP poisoning attacks
- I manage F5 WAF which operates at Layer 7 (Application)
  and Radware DefensePro at Layers 3 & 4
- Understanding TCP handshake helps me identify 
  SYN flood DDoS attacks in network traffic
- VLAN segmentation is used in our infrastructure to 
  separate departments — exactly as described here
- Stateful vs Stateless firewall concepts apply directly 
  to our F5 WAF and perimeter firewall configurations

## Key Takeaway
Networking fundamentals are the backbone of SOC work. 
You cannot detect or investigate threats without 
understanding how data travels across networks — 
from physical cables all the way up to application 
layer protocols.
