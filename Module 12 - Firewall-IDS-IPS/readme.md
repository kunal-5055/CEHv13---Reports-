# Module 12 — Firewalls, IDS & IPS 🛡️🔒

<img src="https://img.shields.io/badge/Network-Security-red?logo=cisco" alt="Network Security Badge" />
<img src="https://img.shields.io/badge/Tool-Snort-orange?logo=snort" alt="Snort" />
<img src="https://img.shields.io/badge/Tool-Suricata-blue?logo=suricata" alt="Suricata" />

**Short description:**
This module explains network- and host-level controls used to enforce security: firewalls (packet, stateful, application), Intrusion Detection Systems (IDS), and Intrusion Prevention Systems (IPS). Includes detection logic, deployment architectures, common tools, and hands-on labs for detection and response.

---

## 🎯 Learning objectives

By the end of this module you should be able to:

* Differentiate between firewall types (packet filter, stateful, application-layer) and their roles.
* Explain IDS vs IPS: detection-only vs inline prevention and placement trade-offs.
* Configure basic rules/signatures for Snort/Suricata and test them in a lab.
* Design simple network segmentation and firewall policies to reduce attack surface.
* Interpret IDS alerts and perform basic incident response (isolate, block, investigate).

---

## 🔑 Key concepts

* **Firewall** — device that filters network traffic based on rules (packet headers, ports, protocols, application semantics).
* **Stateful inspection** — tracking connection state (e.g., TCP handshake) to allow/deny packets.
* **Application Layer (WAF)** — inspects application traffic (HTTP) for attacks like SQLi, XSS.
* **IDS (Intrusion Detection System)** — monitors traffic and raises alerts (NIDS/NIPS, HIDS).
* **IPS (Intrusion Prevention System)** — placed inline to block malicious traffic automatically.
* **Signatures vs Anomaly detection** — pattern-based rules vs behavioral baselines.
* **False positives / false negatives** — tuning and risk trade-offs.

---

## ⚡ Types & deployment

* **Network-based Firewall (NAT, packet filter)** — usually at perimeter, controls IP/port/protocol.
* **Stateful firewall** — tracks sessions to make richer decisions.
* **Next-Generation Firewall (NGFW)** — integrates application awareness, user identity, TLS inspection.
* **Host-based firewall (HFW)** — software firewalls on endpoints (e.g., iptables, Windows Firewall).
* **Network IDS (NIDS)** — monitors mirrored/span traffic (promiscuous), passive alerting.
* **Network IPS (NIPS)** — inline, can drop/reset traffic.
* **Host IDS (HIDS)** — monitors host events, logs, integrity (e.g., OSSEC, Wazuh).

---

## 🛠 Tools commonly used in labs

(Use in authorized, isolated lab environments.)

* Snort — signature-based NIDS/NIPS.
* Suricata — high-performance IDS/IPS with multi-threading.
* Zeek (formerly Bro) — network analysis and logging (protocol parsing).
* iptables / nftables (Linux) — host-level firewall configuration.
* pfSense / OPNsense — open-source firewall appliances for lab networks.
* Wazuh / OSSEC — host-based monitoring & alerts.
* tcpdump / Wireshark — packet capture for analysis.

---

## 🖥 Safe lab setup (recommended)

1. Create an isolated lab network (VMs + virtual switch) or use physical testbed.
2. Deploy a vulnerable service (simple web app) and a gateway VM running pfSense/OPNsense.
3. Mirror traffic to an IDS VM (Snort/Suricata) or place an inline IPS for prevention exercises.
4. Generate benign and malicious traffic using tools like `curl`, `sqlmap`, `nmap`, and custom scripts.

---

## 🧪 Practical exercises (suggested)

### Lab A — Basic firewall rules (pfSense / iptables)

1. Implement rules to allow only HTTP (80) and SSH (22) to a web host, block other inbound ports.
2. Test with `nmap` from attacker VM and observe which ports are reachable.

**Learning point:** restrictive default-deny policies reduce attack surface.

### Lab B — Deploy Snort/Suricata as NIDS

1. Install Suricata on monitoring VM and enable EVE JSON logging.
2. Load sample rule to detect suspicious HTTP requests (e.g., SQLi signature).
3. From attacker VM, run a SQL injection test against lab web app; observe alert generation.

**Learning point:** signature-based detection can rapidly identify known malicious patterns.

### Lab C — Inline IPS blocking

1. Place Suricata or Snort inline as an IPS in the traffic path.
2. Configure a drop action for a known malicious signature.
3. Attempt the same attack and verify the IPS blocks the request and logs the event.

**Learning point:** prevention requires careful tuning to avoid disrupting legitimate traffic.

### Lab D — Host-based IDS (HIDS)

1. Install Wazuh/OSSEC on a target host and configure file integrity monitoring (FIM).
2. Make a benign config change and then a suspicious change (e.g., add a new user or modify `sudoers`) and observe alerts.

**Learning point:** HIDS helps detect compromises that network sensors may miss.

---

## 🛡 Best practices & hardening

* Default-deny firewall posture for inbound access; allow only required services.
* Segment networks by trust level (DMZ, internal, management) and apply strict ACLs between zones.
* Keep IDS/IPS signatures updated and tune to reduce false positives.
* Use TLS inspection carefully (privacy and performance trade-offs) to inspect encrypted traffic.
* Centralize logs (SIEM) and correlate IDS alerts with host logs.
* Test rule changes in a staging environment before production deployment.
* Regularly review firewall rules and remove stale/unused rules.

---

## 🔍 Detection & incident response

* Prioritize alerts by severity and source confidence; investigate high-confidence alerts first.
* On detection of an intrusion: isolate affected host/segment, capture network traffic and host artifacts, and block attacker C2 if possible.
* Preserve logs and evidence; update IDS signatures and firewall rules to prevent repeat attacks.
* Perform post-incident review and adjust monitoring/controls.

---

## 📚 Further reading & references

* Snort & Suricata official documentation
* Zeek Network Security Monitor
* CIS network security benchmarks
* pfSense / OPNsense documentation

---

## 📝 Assessment / quiz (suggested)

1. What is the primary difference between an IDS and an IPS?
2. Why is network segmentation important for limiting attacker movement?
3. Describe two challenges of deploying TLS inspection at scale.
4. How would you tune an IDS to reduce false positives without missing real attacks?

---

## ⚖️ Ethical & legal notice

This module is for defensive learning and authorized testing in controlled environments. Do not deploy attack techniques against systems you do not own or have explicit permission to test.

---

*Document created for Module 12 — Firewalls, IDS & IPS.*

