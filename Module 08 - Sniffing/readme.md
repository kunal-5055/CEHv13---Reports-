# 🕵️‍♂️ Sniffing

## 📖 Overview

Network sniffing (also called packet capturing or network traffic analysis) is the process of intercepting and inspecting packets traveling across a network. It is used by network engineers and security professionals for troubleshooting, performance monitoring, intrusion detection, and forensic investigations.

> ⚠️ **Important:** Packet sniffing can capture sensitive information (passwords, session tokens, personal data). Only perform sniffing in environments where you have explicit authorization. Unauthorized interception is illegal and unethical.

---

## 🔍 Types of Sniffing

1. **👀 Passive Sniffing**

   * Captures traffic without altering the network.
   * Works well on networks using hubs or on mirrored SPAN ports.
   * Harder to detect.

2. **⚡ Active Sniffing**

   * Involves techniques that manipulate network traffic to capture data (e.g., ARP spoofing / ARP poisoning, DHCP spoofing).
   * More powerful on switched networks but more likely to be detected and is intrusive.

---

## 🎯 Common Use-Cases

* 🛠️ Troubleshooting application or network problems.
* 📊 Monitoring bandwidth and traffic patterns.
* 🔎 Detecting suspicious or malicious activity.
* 🚨 Incident response and digital forensics.
* 🧪 Research and protocol analysis.

---

## 🧰 Tools

* **Wireshark** 🐬 — GUI packet analyzer for deep inspection and protocol dissection.
* **tcpdump** 📜 — Command‑line packet capture and filtering.
* **tshark** 🦈 — Command‑line version of Wireshark.
* **ettercap / bettercap** 🎭 — Active network manipulation and MITM frameworks for testing (use ethically).
* **ngrep** 🔍 — Pattern matching on network packets.
* **Scapy** 🐍 — Python library for crafting and analyzing packets.

---

## 🧪 Lab Setup & Best Practices

1. 🖥️ **Isolate your lab**: Use virtual machines and internal virtual networks. Never test on production networks.
2. 🔌 **Use network taps or SPAN/mirror ports**: On switches, use port mirroring (SPAN) or a physical tap to capture traffic without disrupting it.
3. 📂 **Capture metadata and full packets when needed**: Save pcap files for later analysis, but be mindful of storage and sensitive data.
4. 📝 **Document everything**: timestamps, commands, packet capture names, and system snapshots.
5. 🎛️ **Use filters while capturing when possible**: reduces file size and eases analysis.

---

## 💻 Example Commands

* Capture all traffic on interface `eth0` to a file:

```bash
tcpdump -i eth0 -w capture.pcap
```

* Capture only HTTP traffic (port 80):

```bash
tcpdump -i eth0 tcp port 80 -w http.pcap
```

* Read a pcap file with tshark and show packet summaries:

```bash
tshark -r capture.pcap -q -z io,phs
```

* Basic Wireshark filter to show only DNS traffic:

```bash
dns
```

---

## 🔎 Analysis Workflow

1. 🗓️ **Identify timeframe & scope** — focus on relevant interfaces, hosts, or time windows.
2. 🧭 **Use high-level filters** — isolate protocols or IP ranges first.
3. 🔗 **Follow streams** — use TCP/UDP stream follow to reconstruct conversations.
4. 📤 **Extract artifacts** — files, credentials, suspicious payloads, or URLs.
5. 📑 **Correlate with logs** — combine pcap analysis with host and network logs for a fuller picture.

---

## 🛡️ Detection & Countermeasures

* 🔒 Enable encrypted protocols (TLS) to protect payloads.
* 🔐 Use port-security and dynamic ARP inspection on switches.
* 👁️ Implement intrusion detection systems (IDS) and network anomaly detection.
* 🕵️ Monitor for ARP table changes and unexpected DHCP servers.

---

## ⚖️ Legal & Ethical Considerations

* ✅ Only capture traffic where you have permission.
* 🔏 Secure captured data (pcaps often contain sensitive info).
* 📜 Follow organizational policies and applicable laws.

---

## 📚 References & Further Reading

* 📘 Wireshark documentation
* 📄 tcpdump man page
* 🐍 Scapy documentation

---

✨ *Part of the CEHv13 Reports project — module: Sniffing.*
