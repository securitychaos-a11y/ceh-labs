# Traffic Analysis — Wireshark

## Lab Overview

This folder covers network **packet capture and traffic analysis** using **Wireshark** — the industry-standard protocol analyzer. Labs focus on understanding normal traffic baselines, identifying attack patterns in PCAP data, and building the skills needed for SOC-level traffic investigation.

Mapped to **MITRE ATT&CK® Discovery (TA0007), Command and Control (TA0011), Exfiltration (TA0010)**.

> PCAPs are captured in an isolated lab environment. `.pcap` and `.pcapng` files are gitignored and kept local due to size and potential sensitivity. Lab write-ups reference specific frame numbers and filters for reproducibility.

---

## Labs in This Folder

| Lab File | Topic | Focus | Status |
|---|---|---|---|
| `lab-01-arp-dns-baseline.md` | Capture and analyze ARP & DNS in a normal network | Protocol basics | 🔲 Not Started |
| `lab-02-detect-nmap-scan.md` | Identify Nmap SYN/TCP/UDP scan signatures in PCAP | Detection | 🔲 Not Started |
| `lab-03-detect-ftp-creds.md` | Capture FTP cleartext credentials | Credential exposure | 🔲 Not Started |
| `lab-04-http-traffic.md` | Analyze HTTP GET/POST, extract transmitted data | Protocol analysis | 🔲 Not Started |
| `lab-05-detect-exploit.md` | Analyze PCAP of MS17-010 exploitation traffic | Attack detection | 🔲 Not Started |
| `lab-06-filter-practice.md` | 20 progressive Wireshark display filter exercises | Skills drill | 🔲 Not Started |

---

## Essential Wireshark Filters — Quick Reference

```
# Filter by IP
ip.addr == 192.168.56.102
ip.src == 192.168.56.101
ip.dst == 192.168.56.104

# Filter by protocol
tcp
udp
dns
http
ftp
arp

# Filter by port
tcp.port == 445
tcp.dstport == 80

# Find SYN packets (Nmap SYN scan signature)
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Find RST packets (closed ports — common in scans)
tcp.flags.reset == 1

# HTTP methods
http.request.method == "POST"

# DNS queries only
dns.flags.response == 0

# Follow TCP stream shortcut: right-click packet → Follow → TCP Stream
```

---

## Lab Template

---

# Lab [##]: [Lab Title]

## Objective

State what you will capture or analyze. Example: *"Generate an Nmap SYN scan against Metasploitable 2 from Kali, capture the traffic with Wireshark on the target interface, and identify the specific packet patterns that distinguish a SYN scan from normal TCP traffic. Document the display filters a SOC analyst would use to triage this alert."*

## Tools Used

- **Wireshark** vX.X.X
- **Capture Interface:** [e.g., `eth0` on Kali / promiscuous mode on virtual switch]
- **Traffic Generator:** [e.g., Nmap from Kali Linux / FTP login from Windows client]
- **Supporting Tools:** [e.g., tshark for CLI filtering]

## Setup / Topology

```
[ Kali Linux ] ──── [ Host-Only vSwitch ] ──── [ Metasploitable 2 ]
  192.168.56.101      (Capture here)             192.168.56.102
```

**Capture setup:**
- Open Wireshark on Kali, select interface `eth0`
- Apply capture filter if needed: `host 192.168.56.102`
- Start capture before generating traffic
- Stop capture after the activity completes

## Steps / Commands

### Step 1 — Start the capture

Start Wireshark on the appropriate interface. Apply a capture filter to limit noise:

```
# Wireshark capture filter (BPF syntax)
host 192.168.56.102 and not arp
```

### Step 2 — Generate the traffic / trigger the event

```bash
# Example: Run an Nmap SYN scan from Kali
nmap -sS 192.168.56.102
```

### Step 3 — Stop capture and analyze

Stop the capture. Apply display filters to isolate relevant traffic:

```
# Show only traffic to/from the target
ip.addr == 192.168.56.102

# Narrow to SYN packets (scan detection)
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Look for SYN-RST patterns (Nmap probing closed ports)
tcp.flags.reset == 1
```

### Step 4 — Follow a stream or export objects

```
# Right-click a packet of interest → Follow → TCP Stream
# File → Export Objects → HTTP (for web traffic labs)
```

**Key frames to document:**
- Frame [X]: [Describe what this frame shows]
- Frame [Y]: [Describe what makes this frame significant]

## Output / Screenshot

> [Embed a Wireshark screenshot showing the filtered view, or paste tshark output]
>
> Annotate what is visible — e.g., "The screenshot shows 1000+ SYN packets with no corresponding ACK from source 192.168.56.101 — a characteristic fingerprint of an Nmap SYN scan."

## Key Takeaways / What Tripped Me Up

- **Takeaway 1:** [What the traffic pattern reveals — how an analyst would recognize this]
- **Takeaway 2:** [A protocol behavior that surprised you or that's easy to miss]
- **What tripped me up:** [e.g., "Wireshark showed 'SYN retransmissions' which I initially flagged wrong — they were actually just closed-port RSTs that Wireshark classified differently"]
- **SOC relevance:** [What SIEM rule or IDS signature would catch this behavior]

## MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| **Tactic** | Discovery (TA0007) |
| **Technique** | Network Service Discovery (T1046) |
| **Detection Opportunity** | High SYN rate from single source, IDS SYN scan rule, NetFlow anomalies, firewall deny logs |

## References

- [Wireshark Display Filter Reference](https://www.wireshark.org/docs/dfref/)
- [MITRE T1046 – Network Service Scanning](https://attack.mitre.org/techniques/T1046/)
- [Malware Traffic Analysis (PCAP practice)](https://malware-traffic-analysis.net/)
- [Wireshark Sample Captures](https://wiki.wireshark.org/SampleCaptures)
