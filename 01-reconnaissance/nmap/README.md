# Reconnaissance — Nmap

## Lab Overview

This folder contains labs focused on passive and active reconnaissance using **Nmap**. Reconnaissance is the first phase of the CEH exam syllabus and maps to the **MITRE ATT&CK® Reconnaissance (TA0043)** tactic.

> All scans are performed against local lab targets (e.g., Metasploitable 2, HackTheBox retired machines, or locally spun-up VMs). No external systems are targeted.

---

## Labs in This Folder

| Lab File | Topic | Status |
|---|---|---|
| `lab-01-host-discovery.md` | Ping sweeps, ARP discovery, ICMP probing | 🔲 Not Started |
| `lab-02-port-scanning.md` | SYN scan, TCP connect, UDP scan | 🔲 Not Started |
| `lab-03-service-version.md` | Service fingerprinting (`-sV`), banner grabbing | 🔲 Not Started |
| `lab-04-os-detection.md` | OS fingerprinting (`-O`), TTL analysis | 🔲 Not Started |
| `lab-05-nse-scripts.md` | NSE scripting engine (vuln, auth, default categories) | 🔲 Not Started |

---

## Lab Template

> Copy this template when creating a new lab write-up in this folder.

---

# Lab [##]: [Lab Title]

## Objective

Clearly state what this lab demonstrates or proves. Example: *"Perform a stealthy SYN scan against a target host to enumerate open TCP ports without completing the three-way handshake, and compare results to a full TCP connect scan."*

## Tools Used

- **Nmap** vX.XX
- **Target OS:** [e.g., Metasploitable 2 / Kali Linux VM]
- **Attacker OS:** [e.g., Kali Linux 2024.x]

## Setup / Topology

Describe the lab environment. Include:

- Network segment (e.g., `192.168.56.0/24` — Host-Only adapter)
- Attacker IP: `192.168.56.101` (Kali)
- Target IP: `192.168.56.102` (Metasploitable 2)
- Isolation method: VMware/VirtualBox Host-Only network (no internet access from target)

```
[ Kali Linux ]───────[ Host-Only vSwitch ]───────[ Metasploitable 2 ]
 192.168.56.101                                    192.168.56.102
```

## Steps / Commands

### Step 1 — [Describe what you're doing]

```bash
# [Brief explanation of what this command does]
nmap -sn 192.168.56.0/24
```

**What to expect:** [Describe expected output before seeing it — forces active understanding]

### Step 2 — [Next action]

```bash
nmap -sS -p 1-1000 -T4 192.168.56.102
```

**Flags explained:**
- `-sS` — SYN (stealth) scan; does not complete the TCP handshake
- `-p 1-1000` — Scan first 1000 ports only
- `-T4` — Aggressive timing; faster, noisier

### Step 3 — [Save output for analysis]

```bash
nmap -sV -oA recon_metasploitable 192.168.56.102
```

*Outputs: `.nmap`, `.xml`, `.gnmap` — Note: `.gnmap` and `.nmap` are gitignored; review locally.*

## Output / Screenshot

> [Embed a trimmed terminal screenshot or paste representative output here]
>
> Example trimmed output:
> ```
> PORT    STATE SERVICE VERSION
> 21/tcp  open  ftp     vsftpd 2.3.4
> 22/tcp  open  ssh     OpenSSH 4.7p1
> 80/tcp  open  http    Apache httpd 2.2.8
> 3306/tcp open mysql   MySQL 5.0.51a
> ```

## Key Takeaways / What Tripped Me Up

- **Takeaway 1:** [Something you learned or confirmed — be specific, e.g., "A SYN scan (-sS) requires root/sudo on Linux because raw socket access is needed."]
- **Takeaway 2:** [A misconception corrected or a behavior that surprised you]
- **What tripped me up:** [An honest note on what confused you initially — this is great for interview discussions]

## MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| **Tactic** | Reconnaissance (TA0043) |
| **Technique** | Active Scanning (T1595) |
| **Sub-technique** | Scanning IP Blocks (T1595.001) / Wordlist Scanning (T1595.003) |
| **Detection Opportunity** | Firewall logs, IDS alerts (Snort/Suricata SYN flood rules), high connection rate from single source |

## References

- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [MITRE T1595 – Active Scanning](https://attack.mitre.org/techniques/T1595/)
- [Nmap NSE Script Database](https://nmap.org/nsedoc/)
