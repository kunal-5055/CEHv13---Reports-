# Module 18 — IoT & OT Hacking 🚀🔌

> **Goal:** Learn how to assess, exploit, and secure Internet-of-Things (IoT) and Operational Technology (OT) devices and systems. This module mixes hardware, firmware, network and protocol analysis with safe, legal lab practice.

---

## Table of contents

1. [Overview](#overview)
2. [Learning objectives](#learning-objectives)
3. [Prerequisites](#prerequisites)
4. [Lab equipment & tools](#lab-equipment--tools)
5. [Course outline & lessons](#course-outline--lessons)
6. [Hands-on labs / exercises](#hands-on-labs--exercises)
7. [Deliverables & assessment](#deliverables--assessment)
8. [Cheat sheet / quick commands](#cheat-sheet--quick-commands)
9. [Safety, ethics & legal](#safety-ethics--legal)
10. [Suggested further reading & learning path](#suggested-further-reading--learning-path)

---

## Overview

IoT/OT devices (smart cameras, industrial PLCs, building automation, sensors, gateways) power homes and critical infrastructure. They combine embedded firmware, specialized protocols (MQTT, Modbus, BACnet, OPC-UA), and physical interfaces (UART, JTAG). This module teaches how attackers find and exploit weaknesses — and how defenders mitigate them.

---

## Learning objectives

By the end of Module 18 you will be able to:

* 🧭 Enumerate and profile IoT/OT devices on a network.
* 🔎 Extract and analyze firmware (static + dynamic).
* 🪛 Interact with physical debug interfaces (UART, JTAG) safely in a lab.
* 🧪 Perform protocol analysis, fuzzing and exploit development for IoT/OT protocols (MQTT, Modbus, BACnet, OPC-UA).
* 🛡 Produce concise remediation guidance and threat models for IoT/OT assets.

---

## Prerequisites

* Comfortable with Linux (bash), basic networking and TCP/IP.
* Familiarity with reverse engineering basics (static analysis with Ghidra/IDA or similar).
* Basic electronics knowledge (soldering, multimeter usage) is **recommended** for hardware labs.
* Previous modules (scanning, web, and firmware basics) helpful but not strictly required.

---

## Lab equipment & tools

**Hardware (recommended):**

* A set of IoT devices (cheap IP camera, Wi-Fi plug, router, industrial PLC simulator or old PLC)
* USB-to-TTL (UART) adapter (e.g., FTDI)
* JTAG / SWD adapter (e.g., Bus Pirate, JTAGulator)
* Logic analyzer (optional)
* Raspberry Pi or small VM lab host

**Software / tools (suggested):**

* `nmap`, `masscan` — network discovery
* `wireshark`, `tshark` — packet capture & protocol analysis
* `binwalk`, `foremost`, `strings` — firmware extraction
* `qemu`, `firmadyne`, `firmware-mod-kit` — emulation & dynamic testing
* `Ghidra`, `radare2` — binary reversing
* `Burp Suite` / `mitmproxy` — http/https interception (for device web UIs / APIs)
* `mosquitto` (MQTT broker), `modbus-cli`, `scapy` — protocol testing
* `impacket` — SMB/NETBIOS testing where relevant
* `metasploit` (optional) — some IoT modules exist
* `flir` / camera tools — for camera-specific testing
* `firmwalker` / `binwalk` plugins — for automated analysis

> Tip: run all risky tests in an isolated lab network (air-gapped or VLAN) to avoid harming production services.

---

## Course outline & lessons

1. **Intro to IoT & OT security** — attack surface, threat model, real-world incidents.
2. **Device discovery & fingerprinting** — nmap scripts, UPnP, SSDP, mDNS, Shodan basics (lab uses local discovery).
3. **Network services & protocol basics** — MQTT, CoAP, Modbus, BACnet, OPC-UA — how they work and common weaknesses.
4. **Firmware lifecycle & extraction** — downloading firmware, unpacking, identifying filesystem, important files (config, keys).
5. **Static firmware analysis** — searching for secrets, credentials, hardcoded keys, web UI issues.
6. **Dynamic firmware analysis & emulation** — QEMU/Firmadyne, chroot & runtime behavior.
7. **Hardware debugging** — UART consoles, JTAG, safe soldering and reading boot logs.
8. **Vulnerabilities & exploit primitives** — buffer overflows, command injection, insecure update mechanisms.
9. **OT-specific considerations** — safe testing against PLCs, simulation environments, fail-safe practices.
10. **Hardening & mitigation** — secure update chains, network segmentation, monitoring, secure boot recommendations.

---

## Hands-on labs / exercises (suggested)

Each lab should include a short report with steps, findings, PoC (screenshots/logs), and remediation.

### Lab A — Device Recon & Fingerprinting 🔎

* Use `nmap` and `masscan` to find devices.
* Use `nmap -sV --script=banner` and examine HTTP/UPnP responses for device model strings.

### Lab B — Firmware Extraction & Static Analysis 🗜

* Download firmware from vendor site or extract from device image.
* Run `binwalk -e firmware.bin`, inspect extracted filesystem.
* Search for credentials: `grep -R "password\|passwd\|secret\|api_key" -n .`

### Lab C — UART Console & Boot Analysis 🪛

* Connect USB-TTL to device UART (GND, TX, RX).
* Observe boot log at 115200/8/N/1 (common) and capture it to a file.
* Identify bootloader (U-Boot) and kernel messages.

### Lab D — Emulation & Dynamic Testing 🧑‍💻

* Use Firmadyne or QEMU to boot firmware userspace.
* Interact with device services locally (HTTP endpoints, MQTT clients).

### Lab E — Protocol Attacks (MQTT/Modbus/BACnet) 🔐

* Intercept MQTT with `mosquitto_sub` / `mosquitto_pub`.
* Test Modbus register reads/writes with `pymodbus` or `modpoll`.
* Fuzz a simple protocol with `scapy`.

### Lab F — Exploit Development (safe lab) 💥

* Identify a web admin command injection / weak auth and create a local PoC (non-destructive).
* Demonstrate responsible disclosure steps and remediation.

---

## Example repo structure

```
module-18-iot-ot/
├─ README.md                ← this file
├─ labs/
│  ├─ lab-a-recon/          ← nmap reports, screenshots
│  ├─ lab-b-firmware/       ← binwalk outputs, notes
│  └─ lab-c-hardware/       ← uart logs, wiring diagram
├─ tools/                   ← helper scripts (e.g., mqtt-test.py)
└─ reports/                 ← final lab reports & remediation docs
```

---

## Deliverables & assessment

For each lab submit:

* Short lab report (max 2 pages) with objective, steps, evidence (logs/screenshots), findings & impact assessment.
* PoC code or commands used (clearly marked non-destructive).
* Remediation checklist and CVSS-style severity estimate (optional).

Grading considers correctness, clarity, ethical handling, and quality of remediation.

---

## Cheat sheet / Quick commands

* Discover devices:
  `sudo masscan -p1-65535 --rate=1000 192.168.10.0/24`
  `sudo nmap -sV -Pn -oA nmap-discovery 192.168.10.0/24`
* Firmware extraction:
  `binwalk -e firmware.bin`
* Strings & credentials:
  `strings filesystem.bin | egrep -i "pass|key|token|admin"`
* Capture UART:
  `screen /dev/ttyUSB0 115200`  *(or use minicom)*
* MQTT subscribe:
  `mosquitto_sub -h 192.168.10.5 -t '#' -v`
* Modbus read:
  `modpoll -m rtu -b 9600 -p none -a 1 /dev/ttyUSB0 1 10`

---

## Safety, ethics & legal ⚖️

* **Always** test only on devices/systems you own or have explicit written permission to test.
* Use isolated lab networks (VLANs / air-gapped) — never test on production OT.
* Keep tests non-destructive unless explicitly allowed. If a test might change device state (rewriting firmware, toggling outputs), warn and get approval.
* Follow responsible disclosure if you discover vendor-impacting vulnerabilities.

---
