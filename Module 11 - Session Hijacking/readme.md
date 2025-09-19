# Module 11 — Session Hijacking

**Short description:**
This module covers session hijacking concepts, attack techniques, defensive controls, detection, and hands-on lab exercises. The focus is on understanding how sessions are created, how attackers can steal or spoof sessions, and how to mitigate these risks in web applications and networks.

---

## Learning objectives

By the end of this module you should be able to:

* Explain what a session is and how session management works in web applications.
* Describe common session hijacking techniques and attack vectors.
* Perform safe, controlled lab exercises demonstrating session attacks using intentionally vulnerable targets (e.g., DVWA, WebGoat).
* Identify and apply mitigations and best practices to prevent session hijacking.
* Detect signs of session compromise and respond appropriately.

---

## Key concepts

* **Session** — a server-side concept used to remember client state across HTTP requests. Typically represented by a session identifier (session ID) stored in a cookie, token, or URL.
* **Session token / session cookie** — the identifier used to map a client to server-side state.
* **Authentication vs. authorization** — authentication establishes identity; authorization controls access.
* **Session lifecycle** — creation, renewal, expiration, invalidation.

---

## Types of session attacks (high level)

* **Session fixation** — attacker forces a known session ID on a victim, then uses it after victim authenticates.
* **Session sidejacking / cookie theft** — attacker captures session cookies (e.g., via packet sniffing on HTTP, XSS, or compromised hosts).
* **Cross-site scripting (XSS) → session theft** — using injected scripts to exfiltrate tokens.
* **Man-in-the-middle (MitM)** — intercepting traffic to read or inject session data.
* **Replay attacks** — reusing captured valid tokens to impersonate a user.
* **Token brute-force or prediction** — guessing weak session IDs.

---

## Tools commonly used in labs

(Use these **only** in controlled, authorised environments.)

* Burp Suite (Proxy, Repeater, Intruder)
* OWASP ZAP
* Wireshark (packet capture / analysis)
* Browser DevTools (Storage, Network)
* Cookie editors / browser extensions
* Vulnerable target platforms: DVWA, WebGoat, Juice Shop

---

## Safe lab setup (recommended)

1. Use an isolated lab VM or environment (e.g., VirtualBox/VMware) that is not connected to production networks.
2. Deploy intentionally vulnerable web apps: DVWA, WebGoat, or Juice Shop.
3. Run attacker tools from a separate VM on the same isolated network.
4. Ensure you have permission for any real-world targets — do *not* test against systems you do not own or have explicit authorization to test.

---

## Practical exercises (suggested)

### Lab A — Observe and steal a session cookie (HTTP, lab only)

1. Deploy DVWA or a simple app configured to use HTTP (not HTTPS) in an isolated lab.
2. From the attacker VM, start Wireshark and capture traffic between victim and server.
3. Log in as a victim user in the victim browser, then filter captured traffic for `Set-Cookie` / `Cookie` headers and observe session ID values.
4. Use the captured cookie in the attacker browser (or via Burp) to access the victim’s session.

**Learning point:** unencrypted session cookies transmitted over HTTP are readable by network eavesdroppers.

### Lab B — Session fixation (DVWA / lab target)

1. Start with an attacker-controlled session ID value.
2. Trick the victim (through social engineering in the lab or a crafted URL) to use that session ID, then have the victim log in.
3. Reuse the session ID from the attacker side to access the authenticated account.

**Learning point:** applications that accept externally-supplied session IDs without regenerating after login are vulnerable.

### Lab C — XSS-based session theft

1. Find a stored/reflected XSS vulnerability in a lab app.
2. Inject a payload that sends `document.cookie` back to an attacker-controlled endpoint.
3. Capture the cookie and use it to impersonate the victim in the lab.

**Learning point:** XSS can lead to full account takeover by stealing session tokens.

---

## Mitigations and best practices

* Always use HTTPS (TLS) for the entire site (including login and authenticated pages).
* Mark cookies `Secure` (only send over TLS) and `HttpOnly` (not accessible via JavaScript) where appropriate.
* Use `SameSite` cookie attribute to limit cross-site cookie sending.
* Regenerate session IDs on authentication (after login) and privilege level changes.
* Implement short session timeouts and inactivity timeouts.
* Bind sessions to client attributes (e.g., IP range or User-Agent) carefully — beware of false positives.
* Use strong, unpredictable session tokens (sufficient entropy, not incremental counters).
* Implement logout endpoints that invalidate server-side session state.
* Protect against XSS (input/output encoding, CSP) to reduce client-side token theft.
* Monitor for unusual session activity (concurrent sessions from different IPs, rapid privilege changes).

---

## Detection & incident response

* Look for anomalous login activity, token reuse, or concurrent sessions from different geolocations.
* If compromise suspected: invalidate session(s), force logout, rotate session keys, reset passwords if needed, and investigate logs.
* Preserve forensic evidence (network captures, logs) for analysis.

---

## Further reading & references

* OWASP Session Management Cheat Sheet
* OWASP Top Ten (relevant items: A2/A3/A5 depending on year)
* DVWA and WebGoat documentation (for lab setup)

---

## Assessment / quiz (suggested)

1. What is the difference between session fixation and session hijacking?
2. Why is `HttpOnly` important for cookies?
3. Describe three defenses that prevent XSS-based session theft.
4. How should a web app behave at login regarding session tokens?

---

## Ethical & legal notice

This material is intended for defensive security education, authorized penetration testing, and learning in controlled environments. Do not use these techniques on systems you do not own or have explicit permission to test. Unauthorized access is illegal and unethical.

---

## Logos & badges

Add visual branding and quick links to make the README look professional. Save icon files in an `assets/` folder (recommended size: 64×64 or 128×128px) and reference them with relative paths so they display on GitHub.

**Suggested assets (place in `assets/`):**

* `assets/owasp.png` — OWASP logo
* `assets/dvwa.png` — DVWA logo
* `assets/webgoat.png` — WebGoat logo
* `assets/juice-shop.png` — OWASP Juice Shop logo
* `assets/burp.png` — Burp Suite logo
* `assets/wireshark.png` — Wireshark logo

**Badge examples (use shields.io):**

```markdown
[![Lab-ready](https://img.shields.io/badge/lab-authorized-green)](#)
[![Tools](https://img.shields.io/badge/tools-Burp%20%7C%20Wireshark-blue)](#tools)
[![License](https://img.shields.io/badge/license-MIT-brightgreen)](LICENSE)
```

**Horizontal icon row (Markdown):**

```markdown
<p align="center">
  <a href="https://owasp.org/"> <img src="assets/owasp.png" alt="OWASP" width="64" height="64"/></a>
  <a href="https://github.com/digininja/DVWA"> <img src="assets/dvwa.png" alt="DVWA" width="64" height="64"/></a>
  <a href="https://owasp.org/www-project-webgoat/"> <img src="assets/webgoat.png" alt="WebGoat" width="64" height="64"/></a>
  <a href="https://github.com/OWASP/juice-shop"> <img src="assets/juice-shop.png" alt="Juice Shop" width="64" height="64"/></a>
  <a href="https://portswigger.net/burp"> <img src="assets/burp.png" alt="Burp Suite" width="64" height="64"/></a>
  <a href="https://www.wireshark.org/"> <img src="assets/wireshark.png" alt="Wireshark" width="64" height="64"/></a>
</p>
```

**Compact table with captions (if you prefer):**

```markdown
| Tool | Purpose |
|---|---:|
| <img src="assets/burp.png" width="48"/> Burp Suite | Web proxy & testing |
| <img src="assets/wireshark.png" width="48"/> Wireshark | Packet capture |
| <img src="assets/owasp.png" width="48"/> OWASP projects | Vulnerable apps & guidance |
```

**How I can help next:**

* I can generate simple 1200×280 banner images (dark cyber theme) and the 64×64 logos optimized for GitHub (PNG or SVG). If you want those, I will create the images and add them into the repo as files.

---

*Document created for Module 11 — Session Hijacking.*

