# Module-03
# How The Web Works — TryHackMe Pre Security

## Rooms Covered
- DNS in Detail
- HTTP in Detail
- How Websites Work
- Putting It All Together
---

## DNS in Detail

**DNS = Domain Name System**
Translates domain names into IP addresses so we don't 
need to remember numbers like 104.26.10.229**Domain Hierarchy:**
- **TLD (Top Level Domain)** — rightmost part (.com, .org, .uk)
  - gTLD = Generic (e.g. .com, .org, .edu, .gov)
  - ccTLD = Country Code (e.g. .in, .co.uk, .ca)
- **Second Level Domain** — main domain name (e.g. tryhackme)
  - Max 63 characters, only a-z, 0-9 and hyphens
- **Subdomain** — sits left of domain (e.g. admin.tryhackme.com)
  - Max 63 characters per subdomain
  - Full domain max length = 253 characters

**DNS Record Types:**

| Record | Purpose |
|--------|---------|
| A | Resolves to IPv4 address |
| AAAA | Resolves to IPv6 address |
| CNAME | Resolves to another domain name |
| MX | Handles email routing for domain |
| TXT | Free text — used for SPF, DMARC, domain verification

**How DNS Resolution Works:**
1. Browser checks local cache
2. Checks Recursive DNS Server (usually provided by ISP)
3. Recursive server checks Root DNS Servers
4. Root server redirects to correct TLD Server
5. TLD Server points to Authoritative Name Server
6. Authoritative Server returns the DNS record
7. Result cached using TTL (Time To Live) value

**Key terms:**
- TTL — how long DNS record is cached (in seconds)
- Recursive DNS Server — provided by ISP, first point of contact
- Authoritative Server — holds actual DNS records for a domain

---

## HTTP in Detail

**HTTP = HyperText Transfer Protocol**
Rules for communicating with web servers (invented by 
Tim Berners-Lee, 1989-1991)

**HTTPS = HTTP Secure**
Encrypted version of HTTP — protects data in transit

**URL Structure:**

- **Host** — domain name or IP of server
- **Port** — 80 for HTTP, 443 for HTTPS
- **Path** — file or resource location
- **Query String** — extra parameters (e.g. ?id=1)
- **Fragment** — specific section on the page


**HTTP Methods:**

| Method | Purpose |
|--------|---------|
| GET | Retrieve information from server |
| POST | Submit data / create new records |
| PUT | Update existing information |
| DELETE | Remove information/records |

**HTTP Status Codes:**

| Code | Meaning |
|------|---------|
| 200 | OK — request successful |
| 201 | Created — new resource created |
| 301 | Moved Permanently — redirect |
| 302 | Found — temporary redirect |
| 400 | Bad Request — something wrong with request |
| 401 | Not Authorised — login required |
| 403 | Forbidden — no permission |
| 404 | Not Found — page doesn't exist |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

**Common Request Headers:**
- Host — which website you want
- User-Agent — browser software and version
- Content-Length — size of data being sent
- Accept-Encoding — compression types supported
- Cookie — stored session data sent back to server

**Common Response Headers:**
- Set-Cookie — stores cookie on client
- Cache-Control — how long to cache response
- Content-Type — type of data returned (HTML, JSON etc.)
- Content-Encoding — compression method used

**Cookies:**
- Small pieces of data stored on your computer
- Used for authentication, session management, preferences
- Sent back to server with every request
- Cookie value is usually a token (not plain text password)

---

## How Websites Work

**Two main components:**
- **Frontend (Client-Side)** — what browser renders 
  (HTML, CSS, JavaScript)
- **Backend (Server-Side)** — server that processes 
  requests and returns responses

**Website Building Blocks:**
- **HTML** — structure and content of the page
- **CSS** — styling and appearance
- **JavaScript** — interactivity and dynamic behaviour

**Key HTML elements:**
- `<!DOCTYPE html>` — defines HTML5 document
- `<head>` — page metadata
- `<body>` — visible content
- `<h1>` — heading, `<p>` — paragraph
- `<img>` — image, `<button>` — button

**Security Issues to note:**
- **Sensitive Data Exposure** — developers sometimes 
  leave credentials or hidden links in HTML source code
  → Always check page source during security assessments
- **HTML Injection** — occurs when unfiltered user input 
  is displayed on page
  → Prevention: always sanitise user input

---

## Putting It All Together

**Full web request flow:**
1. User types URL in browser
2. DNS resolves domain to IP address
3. Browser sends HTTP request to web server
4. Request may pass through Load Balancer first
5. WAF checks request for malicious patterns
6. Web server processes request
7. Backend code runs if needed (PHP, Python etc.)
8. Database queried if required
9. Response sent back to browser
10. Browser renders HTML, CSS, JavaScript

**Key web infrastructure components:**

| Component | Purpose |
|-----------|---------|
| Load Balancer | Distributes traffic, provides failover, does health checks |
| CDN | Hosts static files on servers worldwide, reduces latency |
| Database | Stores and retrieves website data (MySQL, MongoDB etc.) |
| WAF | Protects web server from attacks, rate limiting, bot detection |

**Web Server Software:**
- Apache, Nginx, IIS, NodeJS
- Default root directory: `/var/www/html` (Linux)
- Can host multiple sites using **Virtual Hosts**

**Backend Languages:**
PHP, Python, Ruby, NodeJS, Perl
- Run server-side — client never sees this code
- Can interact with databases, process user input, 
  call external services

## Key Takeaway
Everything on the web runs on HTTP — understanding 
DNS, HTTP methods, status codes, headers and cookies 
is essential for any SOC analyst investigating web-based 
attacks, phishing domains or application vulnerabilities.