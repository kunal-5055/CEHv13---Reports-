# ⚠️ Module 10 – Denial of Service 

Denial of Service (DoS) and Distributed Denial of Service (DDoS) attacks aim to make a service, network, or application unavailable to legitimate users by overwhelming resources or exploiting protocol/implementation weaknesses. This module explains attack types, real-world examples, detection techniques, and mitigation strategies.

---

## 📌 Topics Covered
- 🧩 DoS vs DDoS — definitions & distinctions  
- 📈 Attack types: volumetric, protocol, application-layer  
- 🛠️ Common vectors: SYN flood, UDP flood, ICMP flood, HTTP(s) floods, DNS amplification, NTP/Chargen reflection  
- ⚙️ Tools & frameworks used in testing (lab only): hping3, LOIC, HOIC, slowloris, stress-ng, Metasploit modules (educational)  
- 🛰️ Botnets & amplification techniques  
- 🔍 Detection & monitoring: traffic baselining, anomaly detection, IDS/IPS signals  
- 🛡️ Mitigation strategies: rate limiting, geo-blocking, blackholing, scrubbing services, WAFs, CDN protection, redundancy & architecture hardening  
- 🧪 Lab exercises: simulated DoS/DDoS in controlled environment, capture & analysis with Wireshark/tcpdump  
- 📚 Legal & ethical considerations for DoS testing (always obtain written permission)

---

## ⚠️ Important Safety & Legal Note
Do **NOT** perform DoS/DDoS attacks against systems you do not own or have explicit written permission to test. Applying the concepts in this module should be limited to controlled lab environments or authorized engagements only.

---

## 🧪 Lab Setup (recommended)
- Use isolated lab network (virtual machines / VLAN)  
- Target VM with controlled service (HTTP server, DNS, etc.)  
- Monitoring VM: Wireshark / tcpdump / ntopng  
- Attacker VM(s): Kali Linux (with ethical testing tools)  
- Optional: simulate distributed traffic using multiple VMs or traffic generators

---

## 🔧 Sample Commands (lab-only examples)
- SYN flood with hping3 (lab):  
  `hping3 -S --flood -V -p 80 <target-ip>`  
- Simple HTTP flood (lab):  
  `while true; do curl -s http://<target-ip>/ >/dev/null; done`  
- Capture traffic with tcpdump:  
  `tcpdump -i eth0 -w dos_capture.pcap`

---

## ✅ Detection & Mitigation Checklist
- Establish normal traffic baselines and alerts  
- Implement rate limiting and connection throttling  
- Deploy CDN / scrubbing / DDoS protection services for public-facing services  
- Harden servers (tcp backlog tuning, SYN cookies) and patch vulnerable services  
- Use WAFs to block application-layer floods  
- Maintain incident response playbook and run tabletop exercises

---

## 📄 CEHv13 Report
This folder contains the **Denial of Service** module report in PDF format.  
File: `module-10-denial-of-service.pdf`

---

## 🔗 Further Reading (suggested)
- RFCs for relevant protocols (TCP/IP, DNS)  
- Vendor documentation for WAF/CDN/DDoS protection (Cloudflare, Akamai, AWS Shield)  
- Academic & industry whitepapers on DDoS amplification and mitigation

---

✨ *Preparedness + architecture + monitoring = resilience. Always test responsibly.*

