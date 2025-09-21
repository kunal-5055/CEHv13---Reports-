# Module 17 — Android Platform Hacking 📱🔐

> **Short:** Hands-on guide to attacking, analysing, and securing Android apps & the Android platform. This module focuses on practical techniques you'll use in mobile VAPT, red-team exercises, and CTFs.

---

## Table of contents

1. [Overview](#overview)
2. [Learning outcomes](#learning-outcomes)
3. [Prerequisites](#prerequisites)
4. [Tools & resources](#tools--resources)
5. [Lab setup](#lab-setup)
6. [Module lessons & exercises](#module-lessons--exercises)
7. [Methodology / Testing workflow](#methodology--testing-workflow)
8. [Common vulnerabilities & exploitation patterns](#common-vulnerabilities--exploitation-patterns)
9. [Hardening & mitigation](#hardening--mitigation)
10. [CTF-style challenges](#ctf-style-challenges)
11. [References & further reading](#references--further-reading)
12. [Legal & safety notice](#legal--safety-notice)

---

## Overview

This module teaches how Android differs from traditional desktop/server platforms, how to find and exploit weaknesses in apps and the underlying OS, and how to responsibly report and mitigate findings.

You'll learn both **static** and **dynamic** analysis techniques and how to chain issues (e.g., insecure storage → local privilege escalation) into impactful compromises.

---

## Learning outcomes

By the end of this module you will be able to:

* Reverse-engineer Android APKs (manifest, smali/dex, resources).
* Perform dynamic analysis and runtime hooking to inspect app behaviour.
* Identify common mobile vulnerabilities: insecure storage, insecure communication, component misconfiguration, improper crypto, broken auth.
* Bypass common client-side protections (root/jailbreak detection, certificate pinning) in lab environments.
* Exploit misconfigurations in Android components (activities, services, broadcast receivers, content providers).
* Test for privilege escalation and sandbox escapes on rooted devices/emulators.

---

## Prerequisites

* Comfortable with Linux command line.
* Basic knowledge of Java/Kotlin and Android app architecture.
* Familiarity with networking and HTTP/HTTPS interception.

---

## Tools & resources (suggested)

* **Android SDK / Android Studio** (AVD emulator)
* **adb** (Android Debug Bridge)
* **apktool** (decode/rebuild APKs)
* **jadx / JADX-GUI** (decompile DEX → Java)
* **smali / baksmali** (smali editing)
* **Frida** (runtime hooking)
* **Objection** (runtime mobile exploration)
* **Burp Suite** (proxy + interceptor)
* **MobSF** (automated static analysis)
* **Drozer** (legacy, but useful for component testing)
* **Magisk, SuperSU** (root management in labs)
* **Genymotion** or **AVD** for emulated devices
* **Metasploit** (Android payloads & modules)

> Tip: Use isolated VMs and emulators for testing. Never test on devices you don't own or without permission.

---

## Lab setup

1. Install Android Studio and create an AVD (x86 images recommended for speed).
2. Enable `adb` access and connect emulator/physical device: `adb devices`.
3. Use an HTTP proxy (Burp) and configure device network to route through it.
4. For privileged tests, prepare a rooted emulator or a device with Magisk.

Quick commands:

```bash
# list devices
adb devices

# install an apk
adb install app-debug.apk

# pull an apk from device
adb shell pm path com.example.app
adb pull /data/app/com.example.app-1/base.apk

# forward device port to local (for API testing)
adb forward tcp:8200 tcp:8200
```

---

## Module lessons & exercises

### 1 — Recon & reconnaissance

* Inspect APK: `apktool d app.apk`.
* Read AndroidManifest.xml for exported components and permissions.
* Use `jadx` to view Java-like source.

**Exercise:** Find `exported=true` activities/services and plan exploit vectors.

### 2 — Static analysis

* Search for hardcoded secrets, insecure crypto, insecure file paths.
* Identify custom schemes, deep links, and implicit intents.

**Exercise:** Locate keys or tokens in resources or strings.

### 3 — Dynamic analysis & instrumentation

* Run app on emulator and intercept traffic via Burp.
* Use Frida to hook sensitive functions (e.g., TLS check, crypto routines).

**Example Frida command:**

```bash
frida -U -f com.example.app -l hook.js --no-pause
```

### 4 — Bypassing protections

* Bypass SSL pinning with Frida/Objection.
* Disable root checks in runtime via hooks.

**Exercise:** Bypass simple certificate pinning and view API calls.

### 5 — Component abuse

* Test exported components: start exported activities, send crafted intents, read/write to content providers.

**Exercise:** Exploit an exported `ContentProvider` with insufficient permissions.

### 6 — Insecure storage & IPC

* Check for sensitive data in shared preferences, external storage, SQLite DBs.
* Examine IPC mechanisms that may leak data to other apps.

### 7 — Privilege escalation & kernel/ROM bugs (advanced)

* On rooted devices/emulators, test for SUID binaries, misconfigured permissions.
* Demo local escalation chains in lab only.

### 8 — Post-exploitation fundamentals

* Persistence techniques, exfiltration channels, and safe cleanup.

**Exercise:** After gaining code execution in an emulator, demonstrate a non-destructive proof-of-concept (POC) like reading a test file.

---

## Methodology / Testing workflow

1. Recon (manifest, permissions, exported components)
2. Static analysis (strings, code, resources)
3. Instrumentation & dynamic analysis (Frida, Burp)
4. Exploit components, chains, or storage issues
5. Privilege escalation (lab-only)
6. Report findings with reproducible steps and remediation

---

## Common vulnerabilities & exploitation patterns

* **Exported components** (activities/services/content providers)
* **Insecure data storage** (SharedPreferences, external storage, databases)
* **Insecure communication** (no TLS, weak TLS, certificate pinning misconfig)
* **Insecure authentication & authorization**
* **Code injection / WebView JS bridges**
* **Insecure use of `FileProvider` or `grantUriPermission`**

---

## Hardening & mitigation (quick checklist)

* Minimise exported components; require permissions for sensitive ones.
* Use Android keystore for secrets; never hardcode sensitive keys.
* Enforce HTTPS and validate certificates server-side.
* Protect IPC: validate callers, use permissions and signatures.
* Apply least privilege for components and file access.
* Enable ProGuard/R8 and obfuscation (not a defence-by-itself).

---

## CTF-style challenges (recommended)

* Broken pinning challenge — intercept and modify API call
* Sensitive storage — find and extract secret from APK
* Intent abuse — start an exported activity to escalate privileges
* Logic flaw — bypass client checks and reach admin-only API

---

## References & further reading

* Official Android developer docs — app security guidance
* Frida & Objection docs
* OWASP Mobile Top 10
* Android Security Bulletin

(Include these in your repo as links or PDFs for students.)

---

## Legal & safety notice ⚠️

All techniques in this module are for **educational and authorized testing only**. Do not test systems or devices you do not own or lack explicit permission to test. Unauthorized testing may be illegal.

---

## Contribution

Found an error or want a new lab? Create a PR or open an issue. Keep exercises safe and reproducible.

---

### Contact

For suggestions: `jawalekunal85@gmail.com`

---

*Made with ❤️ for learning mobile security.*

