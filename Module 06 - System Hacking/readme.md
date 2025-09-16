# 🔓 System Hacking

## 📘 Overview

System Hacking covers techniques used by attackers and penetration testers to gain unauthorized access to systems, escalate privileges, maintain persistence, and cover tracks. This module presents offensive techniques alongside defensive controls and detection strategies — intended for ethical testing within an authorized lab or engagement.

---

## 🎯 Objectives

* Learn the full lifecycle of a system compromise: reconnaissance → exploitation → post-exploitation.
* Practice local and remote exploitation techniques on Windows and Linux.
* Master privilege escalation methods and detection of persistence mechanisms.
* Produce clear findings, remediation steps, and risk-based recommendations.

---

## 🔑 Key Topics

* Footprinting & enumeration (OS, users, services, shares)
* Remote and local vulnerability exploitation
* Password attacks (brute-force, credential stuffing, cracking)
* Privilege escalation (Windows & Linux methods)
* Persistence mechanisms and defensive bypasses
* Post-exploitation (credential harvesting, lateral movement)
* Log tampering & anti-forensics (ethical demonstration only)
* Detection & mitigation (EDR/AV, SIEM, hardening)

---

## 🛠️ Tools (common)

* **Metasploit Framework** — exploitation and payload management
* **Nmap / NSE** — service discovery and vulnerability checks
* **Hydra / Medusa / John / Hashcat** — credential attacks
* **Mimikatz** — Windows credential extraction (lab-only)
* **PowerShell / Empire** — post-exploitation scripting
* **Responder / smbmap / smbclient** — SMB / LLMNR / NetBIOS enumeration
* **LinPEAS / WinPEAS** — privilege escalation discovery
* **Veil / msfvenom** — payload generation (ethical testing)
* **Volatility** — memory forensics

---

## 🧭 Lab Methodology (recommended workflow)

1. **Scope & Authorization** — obtain written permission; define rules of engagement.
2. **Recon & Enumeration** — fingerprint OS, open ports, running services, users, and shares.
3. **Initial Access** — exploit vulnerable service or use valid credentials to obtain a shell.
4. **Privilege Escalation** — enumerate local misconfigurations, SUID bits, weak services, kernel issues, and credential reuse.
5. **Post-Exploitation** — dump credentials, gather sensitive files, identify pivot paths.
6. **Persistence** — demonstrate controlled persistence artifacts (documented & reversible).
7. **Cleanup & Reporting** — remove test artifacts if permitted and produce a comprehensive report.

---

## 🧪 Example Commands & Quick Reference

> **Note:** Run only in your lab or against authorized targets.

**Enumeration**

```bash
# Nmap quick host + services
nmap -sC -sV -oN nmap-services.txt <target>

# SMB enumeration (Linux)
smbclient -L //<target> -N
smbmap -H <target>
```

**Windows credential dump (lab only)**

```powershell
# Mimikatz (run in-memory on a test VM)
privilege::debug
sekurlsa::logonpasswords
```

**Privilege escalation (Linux)**

```bash
# Find SUID files
find / -perm -4000 -type f 2>/dev/null
# Run LinPEAS to identify escalation vectors
./linpeas.sh
```

**Post-exploitation persistence (example, lab only)**

```powershell
# Create a scheduled task on Windows (example)
schtasks /create /sc onlogon /tn "Updater" /tr "C:\Windows\Temp\updater.exe"
```

---

## 🛡️ Detection & Mitigation (high level)

* Enforce **least privilege** and restrict administrative accounts.
* Apply timely **patching** and vulnerability management.
* Deploy and tune **EDR** and centralize logs to a SIEM.
* Monitor for credential dumps, unusual process spawning, and suspicious persistence changes.
* Harden network services (restrict SMB, disable unused services, secure RDP).

---

## 📄 Deliverables (what to include in a report)

* Executive summary (non-technical overview)
* Detailed findings with proof-of-concept (screenshots/logs) — redact sensitive data
* Risk ratings and CVSS-like severity guidance
* Step-by-step remediation for each finding
* Timeline of actions taken during the engagement

---

## ⚠️ Legal & Ethical Notice

Only perform system hacking exercises on systems you own or have **explicit written authorization** to test. Unauthorized use of these techniques is illegal and unethical.

---

## 📚 Further Practice & Resources

* Build isolated lab VMs (Kali, Metasploitable, Windows vulnerable boxes)
* Use LinPEAS/WinPEAS to correlate findings with CVEs and recommended fixes
* Practice detection use-cases: generate alerts and write SIEM rules for credential dumping and persistence

---

*Prepared for hands-on learning and professional assessments. Keep all activities ethical and within defined scope.*
