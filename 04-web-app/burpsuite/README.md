# Web Application Testing — Burp Suite

## Lab Overview

This folder covers web application security testing using **Burp Suite Community/Professional Edition**. Labs align with the **OWASP Top 10** vulnerability categories and use intentionally vulnerable applications: **DVWA (Damn Vulnerable Web Application)**, **WebGoat**, and **OWASP Juice Shop**.

Mapped to **MITRE ATT&CK® Initial Access (TA0001), Collection (TA0009), Credential Access (TA0006)**.

> All testing is performed against locally hosted intentionally vulnerable applications. No production or external web applications are targeted.

---

## Labs in This Folder

| Lab File | Topic | OWASP Category | Status |
|---|---|---|---|
| `lab-01-proxy-intercept.md` | Setting up Burp proxy, intercepting HTTP/HTTPS | Setup | 🔲 Not Started |
| `lab-02-sql-injection.md` | Manual SQLi with Burp Repeater (DVWA Low/Med/High) | A03 – Injection | 🔲 Not Started |
| `lab-03-xss-reflected.md` | Reflected XSS identification and exploitation | A03 – Injection | 🔲 Not Started |
| `lab-04-brute-force-intruder.md` | Login brute-force using Burp Intruder | A07 – Auth Failures | 🔲 Not Started |
| `lab-05-idor.md` | Insecure Direct Object Reference exploitation | A01 – Broken Access | 🔲 Not Started |
| `lab-06-csrf.md` | CSRF token analysis and bypass | A05 – Security Misconfig | 🔲 Not Started |

---

## Lab Template

---

# Lab [##]: [Lab Title]

## Objective

State the web vulnerability being tested and what you aim to achieve. Example: *"Use Burp Suite Repeater to manually test for SQL injection in DVWA's login form at Low security level, extract database version and user information, then observe how the Medium security level input validation changes the attack."*

## Tools Used

- **Burp Suite** Community/Pro vX.X
- **Target Application:** [e.g., DVWA v2.3 hosted on Metasploitable 2]
- **Browser:** Firefox with FoxyProxy configured
- **Supporting Tools:** [e.g., sqlmap for validation only]

## Setup / Topology

Describe the web app setup:

- Burp Proxy listener: `127.0.0.1:8080`
- Target URL: `http://192.168.56.102/dvwa/`
- DVWA Security Level: **Low** (confirm in DVWA settings before starting)
- Browser proxy: Configured via FoxyProxy to route traffic through `127.0.0.1:8080`

**Pre-lab checklist:**
- [ ] Burp Suite running, proxy listener active on port 8080
- [ ] Firefox proxy set to `127.0.0.1:8080`
- [ ] Burp CA certificate installed in Firefox (for HTTPS interception)
- [ ] DVWA accessible and security level set correctly
- [ ] DVWA logged in as `admin` / `password`

## Steps / Commands

### Step 1 — Intercept and inspect a request

Navigate to the target form in Firefox with Burp Intercept ON.

*In Burp Proxy → Intercept tab:*
- Observe the raw HTTP request captured
- Note the parameters being sent (form fields, cookies, headers)

```
POST /dvwa/vulnerabilities/sqli/ HTTP/1.1
Host: 192.168.56.102
...
id=1&Submit=Submit
```

### Step 2 — Send to Repeater and test

Right-click the intercepted request → **Send to Repeater** (Ctrl+R).

In the Repeater tab, modify the `id` parameter:

```
id=1' OR '1'='1
```

Click **Send** and observe the response. Look for:
- Database error messages (SQL syntax visible)
- Extra rows returned
- Behavioral difference from a normal request

### Step 3 — Extract information

```
# Test for database version
id=1' UNION SELECT null, version()--
```

```
# Test for current user
id=1' UNION SELECT null, user()--
```

## Output / Screenshot

> [Embed a screenshot of Burp Repeater showing the injected request and the database response]
>
> Note key observations from the response:
> - Response code: `200 OK`
> - Body included: `MySQL version X.X.XX`
> - Visible evidence of successful injection

## Key Takeaways / What Tripped Me Up

- **Takeaway 1:** [What you learned about this vulnerability class]
- **Takeaway 2:** [Observation about how the app's security level affected the attack — what changed at Medium vs. Low?]
- **What tripped me up:** [e.g., "UNION SELECT column count had to match the original query — took a few tries to figure out the number of columns with ORDER BY probing"]
- **Defensive note:** [WAF signatures, parameterized queries, prepared statements, input validation]

## MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| **Tactic** | Initial Access (TA0001) / Collection (TA0009) |
| **Technique** | Exploit Public-Facing Application (T1190) |
| **Detection Opportunity** | WAF logs, SQL error responses in app logs, anomalous query patterns, IDS signatures for SQLi payloads |

## References

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [PortSwigger Web Security Academy – SQL Injection](https://portswigger.net/web-security/sql-injection)
- [MITRE T1190 – Exploit Public-Facing Application](https://attack.mitre.org/techniques/T1190/)
- [DVWA GitHub](https://github.com/digininja/DVWA)
