# Scanning & Enumeration

## Lab Overview

This folder covers **active scanning and service enumeration** — the phase following initial reconnaissance where an attacker (or penetration tester) identifies open ports, running services, versions, and potential vulnerabilities on discovered hosts.

Mapped to **MITRE ATT&CK® Discovery (TA0007)**.

> All scans are performed against isolated lab targets. No external or production systems are targeted.

---

## Labs in This Folder

| Lab File | Topic | Status |
|---|---|---|
| `lab-01-banner-grabbing.md` | Manual banner grabbing with Netcat and Telnet | 🔲 Not Started |
| `lab-02-smb-enumeration.md` | SMB enumeration with enum4linux and smbclient | 🔲 Not Started |
| `lab-03-snmp-enumeration.md` | SNMP community string brute-force, MIB walking | 🔲 Not Started |
| `lab-04-dns-enumeration.md` | Zone transfers, subdomain enumeration, dig/nslookup | 🔲 Not Started |
| `lab-05-nikto-web-scan.md` | Web server scanning with Nikto | 🔲 Not Started |

---

## Lab Template

---

# Lab [##]: [Lab Title]

## Objective

State clearly what enumeration technique is being practiced and what information you aim to extract. Example: *"Perform SMB enumeration against a Windows target to identify shared resources, user accounts, and domain/workgroup information without authentication."*

## Tools Used

- **[Primary Tool]** vX.XX
- **Supporting Tools:** [e.g., Netcat, enum4linux, dig]
- **Target OS:** [e.g., Windows Server 2019 / Metasploitable 2]
- **Attacker OS:** [e.g., Kali Linux 2024.x]

## Setup / Topology

Describe the lab network layout:

- Attacker IP: `192.168.56.101` (Kali Linux)
- Target IP: `192.168.56.103` (Windows Server / Metasploitable)
- Network: VMware Host-Only (`192.168.56.0/24`)

```
[ Kali Linux ]───────[ Host-Only vSwitch ]───────[ Target Host ]
 192.168.56.101                                    192.168.56.103
```

## Steps / Commands

### Step 1 — [Initial scan to identify relevant ports]

```bash
# First confirm the service is open before enumerating
nmap -p 445,139 --open 192.168.56.103
```

### Step 2 — [Enumerate the service]

```bash
# Run enum4linux for full SMB enumeration
enum4linux -a 192.168.56.103
```

**Key flags:**
- `-a` — Run all basic enumeration checks (users, shares, groups, OS info)

### Step 3 — [Follow-up / deeper probe]

```bash
# List SMB shares without credentials (null session)
smbclient -L //192.168.56.103 -N
```

## Output / Screenshot

> [Paste trimmed, representative output from your terminal]
>
> Example:
> ```
>  Sharename       Type      Comment
>  ---------       ----      -------
>  ADMIN$          Disk      Remote Admin
>  C$              Disk      Default share
>  IPC$            IPC       Remote IPC
> ```

## Key Takeaways / What Tripped Me Up

- **Takeaway 1:** [Specific insight from this lab]
- **Takeaway 2:** [Behavior that surprised you or a gotcha]
- **What tripped me up:** [Honest note on a stumbling block — great for interview prep]

## MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| **Tactic** | Discovery (TA0007) |
| **Technique** | Network Service Discovery (T1046) |
| **Detection Opportunity** | Windows Event ID 4624/4625 (logon attempts), SMB access logs, IDS signature for null sessions |

## References

- [MITRE T1046 – Network Service Scanning](https://attack.mitre.org/techniques/T1046/)
- [enum4linux GitHub](https://github.com/CiscoCXSecurity/enum4linux)
