# 🛡️ CEH Labs — Hands-On Ethical Hacking Practice

> **Target Certification:** Certified Ethical Hacker (CEH) v13  
> **Focus Area:** Offensive security fundamentals, threat simulation, and SOC-oriented detection awareness  
> **Author:** Aadhithyan K M — Aspiring Tier 1 SOC Analyst | Cloud Security Focus  

---

## About This Repository

This repository documents my hands-on lab work while preparing for the CEH certification. Each lab is conducted in an isolated, legal home lab environment (no real-world systems are targeted). The goal is not just to pass an exam — it is to build practical skills in the offensive techniques that SOC analysts must recognize, detect, and respond to.

Labs are mapped to the **MITRE ATT&CK® framework** where applicable. Notes on detection opportunities and defensive takeaways are included alongside offensive techniques to reinforce a defender's mindset.

---

## 🧰 The "Core 5" Toolkit

These are the primary tools covered across all labs:

| Tool | Primary Use | Lab Folder |
|---|---|---|
| **Nmap** | Network discovery, port scanning, OS/service fingerprinting | `/01-reconnaissance/nmap/` |
| **Metasploit Framework** | Exploitation, post-exploitation, payload generation | `/03-exploitation/metasploit/` |
| **Burp Suite** | Web application interception, scanning, manual testing | `/04-web-app/burpsuite/` |
| **Wireshark** | Packet capture, traffic analysis, protocol dissection | `/06-traffic-analysis/wireshark/` |
| **Hashcat** | Offline password cracking, hash identification | `/05-password-attacks/hashcat/` |

---

## 📊 Lab Progress Tracker

| # | Lab | Topic | ATT&CK Tactic | Status | Date Completed |
|---|---|---|---|---|---|
| 01 | Nmap Host Discovery | Reconnaissance | TA0043 – Reconnaissance | 🔲 Not Started | — |
| 02 | Nmap Service & Version Scan | Scanning & Enumeration | TA0007 – Discovery | 🔲 Not Started | — |
| 03 | Nmap NSE Script Scanning | Scanning & Enumeration | TA0007 – Discovery | 🔲 Not Started | — |
| 04 | Metasploit: EternalBlue (MS17-010) | Exploitation | TA0002 – Execution | 🔲 Not Started | — |
| 05 | Metasploit: Meterpreter Post-Exploitation | Post-Exploitation | TA0004 – Privilege Escalation | 🔲 Not Started | — |
| 06 | Burp Suite: Intercepting HTTP Traffic | Web App Testing | TA0043 – Reconnaissance | 🔲 Not Started | — |
| 07 | Burp Suite: SQL Injection (DVWA) | Web App Attacks | TA0009 – Collection | 🔲 Not Started | — |
| 08 | Hashcat: MD5 / NTLM Hash Cracking | Password Attacks | TA0006 – Credential Access | 🔲 Not Started | — |
| 09 | Wireshark: ARP & DNS Analysis | Traffic Analysis | TA0007 – Discovery | 🔲 Not Started | — |
| 10 | Wireshark: Detecting Nmap Scans | Detection Lab | TA0007 – Discovery | 🔲 Not Started | — |

**Status Key:** 🔲 Not Started · 🔄 In Progress · ✅ Complete

---

## 📁 Repository Structure

```
ceh-labs/
├── 01-reconnaissance/
│   └── nmap/
├── 02-scanning-enumeration/
├── 03-exploitation/
│   └── metasploit/
├── 04-web-app/
│   └── burpsuite/
├── 05-password-attacks/
│   └── hashcat/
├── 06-traffic-analysis/
│   └── wireshark/
└── notes/
    └── mitre-attack-mapping.md
```

---

## 🗺️ Study Roadmap

My CEH study approach follows this sequence:

1. **Footprinting & Reconnaissance** — Passive/active info gathering, Nmap fundamentals
2. **Scanning & Enumeration** — Port scanning strategies, service fingerprinting, banner grabbing
3. **Exploitation** — Vulnerability research, Metasploit framework, payload types
4. **Web Application Attacks** — OWASP Top 10 hands-on (DVWA, WebGoat), Burp Suite workflows
5. **Password Attacks** — Hash identification, dictionary attacks, rainbow tables
6. **Traffic Analysis** — Capture and dissect attack traffic, identify IOCs in PCAP
7. **MITRE ATT&CK Mapping** — Map each lab technique to ATT&CK tactics and techniques

---

## 🚀 How to Use This Repository

1. **Browse by phase** — Each numbered folder corresponds to a phase of the CEH exam and real-world attack lifecycle.
2. **Read the lab README** — Every lab folder contains a `README.md` with objective, setup, commands, and takeaways.
3. **Review notes/** — The `/notes/` folder contains MITRE ATT&CK mapping references and general write-ups useful for interview prep.
4. **Screenshots** — Output screenshots are embedded in lab READMEs where applicable (stored locally, not committed to keep repo size small).

> ⚠️ **Disclaimer:** All techniques demonstrated in this repository are performed in isolated, legal lab environments (local VMs, intentionally vulnerable systems such as Metasploitable, DVWA, VulnHub VMs). Do not use these techniques against systems you do not own or have explicit written permission to test.

---

## 📬 Contact

Aadhithyan K M — [linkedin.com/in/aadhithyan-km](https://www.linkedin.com/in/aadhithyan-km-86613a345) — [github.com/securitychaos-a11y](https://github.com/securitychaos-a11y)
