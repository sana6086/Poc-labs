# Poc-labs
Penetration testing &amp; VAPT lab write-ups — web application, API, Android, and red team security research. Structured reports with methodology, PoC, and remediation guidance.

> All targets represented here are intentionally vulnerable practice
> machines or authorized lab platforms. No client, employer, or
> third-party production systems are represented in this repository.

---

## Web Application

| Lab | Vulnerability Class | Severity |
|---|---|---|
| [Temple of Doom](./web/temple-of-doom/) | Insecure Deserialization → RCE | High |
| [Mr. Robot](./web/Mr.%20Robot/) | CMS Misconfiguration → Privilege Escalation | High |
| [SQLi-Database-Exfilteration](./web/SQLi-Database-Exfilteration) | SQL Injection | High |

## API Security

| Lab | Vulnerability Class | Severity |
|---|---|---|
| [DVAPI](./API-Testing/DVAPI/) | BOLA / BFLA / JWT Mismanagement | High |

## Android Security

| Lab | Vulnerability Class | Severity |
|---|---|---|
| [BUGbazaar - Static APK Testing](./APK-Testing/BUGbazaar/) | Static Analysis | Medium |
| [BUGbazaar — Dynamic APK Testing](./APK-Testing/BUGbazaar/) | Runtime Manipulation, SSL Pinning Bypass | Medium |

## Red Team & CTF

| Lab | Category | Result |
|---|---|---|
| [Kioptrix](./redteam/kioptrix/) | Boot-to-Root | Root obtained |
| [Password Cracking](./redteam/password-cracking/) | Credential Attacks | — |
| [Network Sniffing](./redteam/network-sniffing/) | Traffic Analysis | — |
| [Social Engineering](./redteam/social-engineering/) | Human-Layer Attack Simulation | — |
| [Footprinting & Information Gathering](./redteam/footprinting-recon/) | Reconnaissance | — |

---

## Methodology

Every engagement in this repo follows the same process:

**Reconnaissance → Enumeration → Attack Surface Mapping → Vulnerability
Discovery → Manual Validation → Controlled Exploitation → Impact
Assessment → Reporting & Remediation.**

Each lab folder contains a `README.md` with the full write-up (severity,
summary, steps to reproduce, proof of concept, remediation guidance)
and the original report as a linked PDF where applicable.

---

## About

Penetration Tester / VAPT Analyst — final-year BS Cybersecurity student
with hands-on internship experience in banking infosec (VAPT, SOC, GRC)
and independent bug-bounty research across web, API, and mobile
platforms.

**Contact:** [LinkedIn](https://linkedin.com/in/sanaabdul-rehman-6a61a8383) · [Portfolio](https://sana6086.github.io) · [Email](mailto:sanarehman6086@gmail.com)
