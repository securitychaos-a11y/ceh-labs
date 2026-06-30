# MITRE ATT&CK® Mapping — CEH Labs

> This document maps every lab in this repository to the corresponding MITRE ATT&CK tactic and technique. Use this for interview preparation — being able to describe offensive techniques in ATT&CK language is a strong differentiator in SOC analyst interviews.

**ATT&CK Navigator Layer:** [Create one at mitre-attack.github.io/attack-navigator/](https://mitre-attack.github.io/attack-navigator/) using the techniques below.

---

## Master Mapping Table

| Lab | Tool | ATT&CK Tactic | ATT&CK Technique | Technique ID | Detection Opportunity |
|---|---|---|---|---|---|
| Host Discovery (Nmap) | Nmap | Reconnaissance | Active Scanning – Scanning IP Blocks | T1595.001 | Firewall deny logs, ICMP anomaly detection |
| Port Scanning (Nmap) | Nmap | Discovery | Network Service Discovery | T1046 | IDS SYN scan alerts, NetFlow high-port-rate |
| Service Fingerprinting (Nmap) | Nmap | Discovery | Network Service Discovery | T1046 | Banner grabbing detection, anomalous probe patterns |
| NSE Script Scanning | Nmap | Discovery | Network Service Discovery | T1046 | Specific NSE probe signatures in IDS |
| SMB Enumeration | enum4linux | Discovery | Account Discovery | T1087 | Windows Event ID 4625, null session attempts |
| DNS Enumeration | dig / nslookup | Reconnaissance | Gather Victim Network Info – DNS | T1590.002 | DNS query volume anomaly, zone transfer attempt logs |
| EternalBlue (MS17-010) | Metasploit | Execution | Exploit Remote Services | T1210 | Windows Event 4688, SMBv1 traffic, IDS EternalBlue signature |
| Meterpreter Post-Exploitation | Metasploit | Privilege Escalation | Exploitation for Privilege Escalation | T1068 | LSASS memory access (Event 10), suspicious child process of svchost |
| SQL Injection | Burp Suite | Initial Access | Exploit Public-Facing Application | T1190 | WAF alerts, SQL error responses in app logs |
| XSS | Burp Suite | Collection | Input Capture | T1056 | CSP violations, anomalous script tags in requests |
| Brute Force – Intruder | Burp Suite | Credential Access | Brute Force – Password Guessing | T1110.001 | Login failure rate (Event 4625), account lockout |
| MD5/NTLM Hash Cracking | Hashcat | Credential Access | Brute Force – Password Cracking | T1110.002 | Offline — defend with bcrypt/Argon2 + MFA |
| ARP/DNS Capture | Wireshark | Discovery | Remote System Discovery | T1018 | ARP sweep detection, DNS query anomalies |
| Detect Nmap Scan (PCAP) | Wireshark | Discovery | Network Service Discovery | T1046 | High SYN rate, RST flood from single source |
| FTP Cleartext Credentials | Wireshark | Credential Access | Network Sniffing | T1040 | Unencrypted protocol use, FTP on port 21 |
| EternalBlue PCAP Analysis | Wireshark | Execution | Exploit Remote Services | T1210 | SMBv1 exploit patterns, LSASS spike after SMB session |

---

## Key ATT&CK Tactics — Quick Reference

| Tactic | ID | Description | Typical Tools Covered |
|---|---|---|---|
| Reconnaissance | TA0043 | Information gathering before attack | Nmap, passive recon |
| Initial Access | TA0001 | Gaining first foothold | Metasploit, Burp Suite (web) |
| Execution | TA0002 | Running malicious code | Metasploit, payloads |
| Persistence | TA0003 | Maintaining access | Metasploit persistence modules |
| Privilege Escalation | TA0004 | Gaining higher permissions | Metasploit, local exploits |
| Credential Access | TA0006 | Stealing credentials | Hashcat, Mimikatz, Wireshark |
| Discovery | TA0007 | Learning the environment | Nmap, enum4linux |
| Lateral Movement | TA0008 | Moving through the network | Metasploit pivot, PsExec |
| Collection | TA0009 | Gathering data | Wireshark, Burp Suite |
| Exfiltration | TA0010 | Moving data out | DNS tunneling, HTTP covert channel |
| Command & Control | TA0011 | Maintaining C2 | Meterpreter, reverse shells |

---

## Resources

- [MITRE ATT&CK Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/)
- [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/)
- [ATT&CK for SOC Analysts (MITRE Blog)](https://medium.com/mitre-attack)
- [Atomic Red Team — Test Implementations](https://github.com/redcanaryco/atomic-red-team)
