# Password Attacks — Hashcat

## Lab Overview

This folder covers **offline password cracking** techniques using **Hashcat** — the world's fastest GPU-based password recovery tool. Labs cover hash identification, dictionary attacks, rule-based attacks, and combinator/mask attacks.

Mapped to **MITRE ATT&CK® Credential Access (TA0006)**.

> All cracking is performed on hashes generated in the local lab environment or from intentionally vulnerable machines (e.g., hashdump from Metasploitable). No production credential hashes are used.

---

## Labs in This Folder

| Lab File | Topic | Hash Type | Status |
|---|---|---|---|
| `lab-01-hash-identification.md` | Identify hash types using hashid and hash-identifier | Various | 🔲 Not Started |
| `lab-02-md5-dictionary.md` | Dictionary attack against MD5 hashes with rockyou.txt | MD5 | 🔲 Not Started |
| `lab-03-ntlm-crack.md` | Crack NTLM hashes dumped from Metasploitable | NTLM | 🔲 Not Started |
| `lab-04-rules-attack.md` | Rule-based attack with best64.rule | MD5 / NTLM | 🔲 Not Started |
| `lab-05-mask-attack.md` | Mask attack for known-format passwords | MD5 | 🔲 Not Started |
| `lab-06-bcrypt.md` | Observe bcrypt cracking difficulty vs. MD5 | bcrypt | 🔲 Not Started |

---

## Lab Template

---

# Lab [##]: [Lab Title]

## Objective

State the cracking goal and what the lab demonstrates. Example: *"Perform a dictionary attack against a set of MD5 password hashes using rockyou.txt, then apply transformation rules with best64.rule to crack passwords that resist a plain dictionary attack. Observe the difference in cracking speed between MD5 and bcrypt."*

## Tools Used

- **Hashcat** vX.X.X
- **Wordlist:** rockyou.txt (`/usr/share/wordlists/rockyou.txt` on Kali)
- **Rules:** best64.rule (bundled with Hashcat)
- **OS:** Kali Linux (CPU mode — GPU preferred but not required for small hash sets)
- **Hash source:** [Generated locally / Obtained from Metasploitable hashdump via Meterpreter]

## Setup / Topology

This lab runs entirely on the Kali attacker machine. No network connectivity to target required for this lab.

**Pre-lab checklist:**
- [ ] `hashcat --version` confirms installation
- [ ] rockyou.txt available (or decompress: `sudo gzip -d /usr/share/wordlists/rockyou.txt.gz`)
- [ ] Hash file prepared: `hashes.txt` (one hash per line)
- [ ] Confirm hash type before selecting Hashcat mode (`-m`)

**Common Hashcat Mode Reference:**

| Hash Type | Hashcat `-m` Value |
|---|---|
| MD5 | `-m 0` |
| SHA-1 | `-m 100` |
| SHA-256 | `-m 1400` |
| NTLM | `-m 1000` |
| bcrypt | `-m 3200` |
| SHA-512crypt (Linux) | `-m 1800` |

## Steps / Commands

### Step 1 — Identify the hash type

```bash
# Install hashid if not present
pip3 install hashid

# Identify hash format
hashid '5f4dcc3b5aa765d61d8327deb882cf99'
```

**Expected output:** `[+] MD5`

### Step 2 — Prepare the hash file

```bash
# One hash per line
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hashes.txt
```

### Step 3 — Dictionary attack

```bash
# Basic dictionary attack: MD5 (-m 0) with rockyou.txt
hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt

# Monitor progress: press 's' during run for status
```

### Step 4 — Rule-based attack (if plain dictionary fails)

```bash
# Apply best64.rule transformations to the wordlist
hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt \
  -r /usr/share/hashcat/rules/best64.rule
```

### Step 5 — View cracked results

```bash
# Display cracked hashes from the potfile
hashcat -m 0 hashes.txt --show
```

## Output / Screenshot

> [Paste the Hashcat result output or embed a terminal screenshot]
>
> Example:
> ```
> 5f4dcc3b5aa765d61d8327deb882cf99:password
>
> Session..........: hashcat
> Status...........: Cracked
> Hash.Mode........: 0 (MD5)
> Time.Started.....: [timestamp]
> Speed.#1.........: 12345.6 kH/s
> Recovered........: 1/1 (100.00%)
> ```

## Key Takeaways / What Tripped Me Up

- **Takeaway 1:** [e.g., "MD5 cracked in under 1 second with rockyou.txt — this is why MD5 should never be used for passwords"]
- **Takeaway 2:** [e.g., "bcrypt with cost factor 12 ran at ~100 H/s on CPU — orders of magnitude slower, illustrating the value of proper password hashing"]
- **What tripped me up:** [e.g., "Selected wrong `-m` mode initially because the hash looked like SHA-1 — hashid confirmed MD5; character length is the key clue"]
- **Defensive note:** [bcrypt/scrypt/Argon2 for password storage, MFA as second factor, account lockout policies]

## MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| **Tactic** | Credential Access (TA0006) |
| **Technique** | Brute Force – Password Cracking (T1110.002) |
| **Detection Opportunity** | Offline cracking produces no network logs; defend by enforcing strong hash algorithms and MFA |

## References

- [Hashcat Wiki](https://hashcat.net/wiki/)
- [MITRE T1110.002 – Password Cracking](https://attack.mitre.org/techniques/T1110/002/)
- [SecLists Wordlists](https://github.com/danielmiessler/SecLists)
- [Hashes.com – Online Hash Identifier](https://hashes.com/en/tools/hash_identifier)
