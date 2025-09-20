# 🚨 Module 13 — Web Server Hacking

## 🔎 Overview

This module teaches how to enumerate, test, and exploit common misconfigurations and vulnerabilities in web servers (Apache, Nginx, IIS and other HTTP servers). It balances safe lab practice with real-world attack patterns and defensive countermeasures.

**Learning goals**

* Identify web server technologies and versions.
* Enumerate open HTTP(S) services and discover content.
* Find misconfigurations (directory listing, default files, weak TLS, mis-set permissions).
* Exploit vulnerabilities (file inclusion, upload flaws, path traversal, SSRF, misconfigured proxies).
* Implement remediation and hardening steps.

---

## 📚 Table of Contents

1. [Prerequisites](#-prerequisites)
2. [Lab setup & scope](#-lab-setup--scope)
3. [Tools](#-tools)
4. [Recon & enumeration](#-recon--enumeration)
5. [Common vulnerabilities & tests](#-common-vulnerabilities--tests)
6. [Exploitation examples (step-by-step)](#-exploitation-examples-step-by-step)
7. [Post-exploitation & persistence](#-post-exploitation--persistence)
8. [Mitigation & hardening checklist](#-mitigation--hardening-checklist)
9. [Further reading & resources](#-further-reading--resources)

---

## ✅ Prerequisites

* Basic Linux command-line skills.
* Familiarity with HTTP, headers, TLS.
* Installed tools (see Tools section).
* A safe lab environment (VMs, isolated networks). **Do not attack systems without explicit authorization.**

---

## 🧰 Tools

* nmap / masscan
* curl, wget, openssl
* Nikto, WhatWeb, Wappalyzer
* Gobuster, Dirb, ffuf
* Burp Suite (Community/Professional)
* Metasploit (optional)
* socat, netcat, tcpdump
* openssh / ssh

---

## 🔍 Recon & Enumeration

### 1. Port & service discovery

```bash
nmap -sV -p- --min-rate 2000 -T4 target.example.com
```

Look specifically for 80/443, 8080, 8443, 8000 and any unusual HTTP ports.

### 2. Technology/Version

```bash
whatweb -v target.example.com
# or
curl -I http://target.example.com
```

Check server headers (`Server`, `X-Powered-By`), but remember headers can be faked.

### 3. Directory & content discovery

```bash
gobuster dir -u http://target.example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50
```

Look for backups, config files (`/backup`, `/config/`, `/.git/`, `/admin`), and common entry points.

### 4. SSL/TLS checks

```bash
openssl s_client -connect target.example.com:443 -servername target.example.com
sslyze --regular target.example.com:443
```

Check for weak ciphers, outdated protocol versions, or expired certs.

---

## ⚠️ Common Vulnerabilities & How to Test

### 1. Directory listing

* Visit folders in browser or use curl.
* If listing is enabled, sensitive files may be exposed.

### 2. Default files & backups

* `index.html.bak`, `config.php~`, `db.sql` often left by developers.
* Search for `.env`, `wp-config.php`, `.git/config`.

### 3. Local File Inclusion (LFI) / Remote File Inclusion (RFI)

* Test parameters that include file names: `?page=`, `?file=`.
* Example payloads:

```
?page=../../../../etc/passwd
?page=php://filter/convert.base64-encode/resource=somefile.php
```

### 4. Path traversal

* Try `../` sequences and encoded sequences `%2e%2e%2f`.

### 5. File upload flaws

* Test upload functionality: set content-type mismatches, double extensions (`shell.php.jpg`), bypass content-checks.
* Use Burp to tamper file metadata and extension.

### 6. Server-side request forgery (SSRF)

* Test input that performs server-side requests (URL fields, image fetchers).
* Probe internal services (127.0.0.1:\*/, 169.254.169.254 for cloud metadata).

### 7. Misconfigured proxy / reverse proxy issues

* Host header poisoning, URL rewriting issues, improper forwarding of client IPs.

### 8. Information disclosure

* Exposed debug pages, stack traces, version endpoints.

---

## 🧪 Exploitation Examples (concise labs)

> These examples are for educational labs ONLY.

### Example A — Directory traversal → local file read

1. Discover a parameter `?view=`.
2. Test traversal:

```
GET /index.php?view=../../../../etc/passwd HTTP/1.1
Host: target.example.com
```

3. If successful, you'll see `/etc/passwd` contents.

### Example B — File upload to get code execution

1. Find upload endpoint (e.g., `/upload.php`).
2. Upload `shell.php` disguised as `shell.php.jpg` and tamper `Content-Type` in Burp.
3. Access uploaded file: `http://target.example.com/uploads/shell.php.jpg` → if server executes PHP on that file, you get RCE.

### Example C — SSRF to access cloud metadata (AWS)

1. Identify SSRF vector (image fetcher, URL preview).
2. Request `http://169.254.169.254/latest/meta-data/` through the server.
3. If reachable, sensitive instance credentials may be exposed.

---

## 🔐 Post-exploitation & Persistence

* If you get a web shell, avoid noisy behavior. Use `ssh` or reverse shells via `socat`/`nc` for interactive sessions.
* Harvest credentials from config files and environment files.
* Check for creds in `/.git`, backups, `wp-config.php`, and application logs.

---

## 🛡️ Mitigation & Hardening Checklist

* Disable directory listing.
* Remove default/demo files and backups from production.
* Enforce strict input validation and output encoding.
* Use secure file upload handling: validate extension, MIME, store outside webroot, rename uploads.
* Harden TLS: only TLS1.2+; strong ciphers; HSTS.
* Disable unnecessary server tokens (`Server`, `X-Powered-By`).
* Use WAF with tuned rules and thorough logging.
* Isolate internal services; block access to instance metadata endpoints from app servers where possible.
* Keep server & modules up to date; remove unused modules.

---

## 📂 Lab Setup Suggestions

* Host: Ubuntu or CentOS VM with Apache, Nginx, and an IIS VM.
* Intentionally vulnerable app: DVWA, Mutillidae, OWASP Juice Shop.
* Use snapshots to revert after each attempt.

---

## 📖 Further reading & resources

* OWASP Testing Guide — Web Application Testing.
* OWASP Top 10 — keep aligned to categories.
* Official docs for Apache/Nginx/IIS hardening.

---

## Contributing

Want improvements? Open a PR with lab additions, commands, or example payloads. Add `assets/` images (badges or logos) to the repo and reference them.

---

## License

This README is free to use under the MIT License. Add a `LICENSE` file if you publish.

---

*Made with ⚡ for Module 13 — Web Server Hacking.*

