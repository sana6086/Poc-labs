# Kioptrix Level 1 — Vulnerability Assessment & Exploitation

> **Network Reconnaissance • Service Enumeration • Vulnerability Identification • Exploitation • Privilege Validation**

## Assessment Overview

This project documents a controlled penetration-testing assessment of the **Kioptrix Level 1** vulnerable virtual machine.

The assessment followed a structured approach consisting of host discovery, port and service enumeration, version identification, vulnerability assessment, controlled exploitation, and privilege validation.

During enumeration, the target was identified as running **Samba 2.2.1a** on a legacy Linux environment. Further assessment identified the **`trans2open`** vulnerability affecting the Samba service. Controlled exploitation successfully established command-shell access and resulted in **root-level privileges** on the target system.

---

## Assessment Objectives

* Perform network reconnaissance against the target environment
* Identify exposed services and attack surfaces
* Enumerate service and software versions
* Identify applicable vulnerabilities
* Validate exploitability in a controlled laboratory environment
* Confirm the resulting privilege level
* Document the attack path and security impact

---

## Environment

| Category                   | Details           |
| -------------------------- | ----------------- |
| Target                     | Kioptrix Level 1  |
| Attacker Platform          | Kali Linux        |
| Target IP                  | `192.168.56.102`  |
| Attacker IP                | `192.168.56.101`  |
| Network                    | `192.168.56.0/24` |
| Primary Vulnerable Service | Samba             |
| Samba Version              | `2.2.1a`          |
| Exploitation Framework     | Metasploit        |
| Identified Vulnerability   | `trans2open`      |
| Final Privilege            | `root`            |

---

## Assessment Methodology

```text
Reconnaissance
      │
      ▼
Port & Service Enumeration
      │
      ▼
Version Identification
      │
      ▼
Vulnerability Assessment
      │
      ▼
Exploit Validation
      │
      ▼
Command Shell
      │
      ▼
Privilege Validation
```

---

## 1. Host Discovery

Initial network reconnaissance was performed against the `192.168.56.0/24` subnet using Nmap.

Four active hosts were identified. The target system was identified as:

```text
192.168.56.102
```

The Oracle VirtualBox MAC address indicated that the host was a virtual machine within the laboratory environment.

---

## 2. Port & Service Enumeration

A targeted Nmap scan identified the following exposed services:

|    Port | Service     | Assessment Relevance  |
| ------: | ----------- | --------------------- |
|  22/tcp | SSH         | Remote administration |
|  80/tcp | HTTP        | Web attack surface    |
| 139/tcp | NetBIOS/SMB | Samba attack surface  |
| 443/tcp | HTTPS       | Secure web service    |

Service-version detection subsequently identified legacy software, including **Samba**, **Apache 1.3.20**, and **OpenSSL 0.9.6b**.

---

## 3. Samba Service Enumeration

Further enumeration was performed against the SMB/Samba service using the Metasploit auxiliary scanner.

The target was identified as running:

```text
Samba 2.2.1a
```

This version information was used to identify applicable vulnerabilities affecting the exposed service.

---

## 4. Vulnerability Identification

Metasploit was used to identify an applicable exploit for the discovered Samba version.

The following module was identified:

```text
exploit/linux/samba/trans2open
```

The `trans2open` vulnerability is a **stack-based buffer overflow** that can allow memory corruption and execution-flow manipulation within vulnerable Samba implementations.

### Finding

**Vulnerability:** Samba `trans2open`
**Affected Service:** Samba 2.2.1a
**Category:** Remote Code Execution / Buffer Overflow
**Severity:** Critical
**Impact:** Potential complete system compromise

---

## 5. Exploitation

The identified vulnerability was validated using the Metasploit Framework in the controlled laboratory environment.

The target and local parameters were configured for the assessment:

```text
RHOSTS = 192.168.56.102
RPORT  = 139
LHOST  = 192.168.56.101
```

A reverse-shell payload was subsequently used to establish command-shell access to the target.

---

## 6. Exploitation Result

Successful exploitation resulted in multiple command-shell sessions being established from the target environment.

The successful exploitation demonstrated that the exposed Samba service could be leveraged to obtain unauthorized command execution on the vulnerable host.

---

## 7. Privilege Validation

The resulting privilege level was validated using:

```text
whoami
```

The system returned:

```text
root
```

This confirmed that exploitation resulted in **root-level access**, representing complete compromise of the target operating system within the laboratory environment.

---

## Security Impact

Successful exploitation of the vulnerable Samba service provided command execution with the highest operating-system privilege level.

An attacker exploiting an equivalent vulnerable and exposed system could potentially:

* Execute arbitrary commands
* Access sensitive files
* Modify system resources
* Compromise application and system data
* Establish persistence
* Further compromise the surrounding environment

The assessment demonstrated the practical impact by obtaining `root` access on the target.

---

## Recommendations

For a production environment, the primary remediation measures would include:

1. **Upgrade or replace the vulnerable Samba implementation** with a supported version.
2. **Remove unsupported legacy software** from production systems.
3. **Restrict SMB/NetBIOS exposure** to trusted networks where the services are required.
4. **Apply security patches regularly** and maintain an asset/software inventory.
5. **Implement network segmentation** to limit the impact of a compromised host.
6. **Monitor SMB activity** for anomalous connections and exploitation attempts.

---

## Tools & Technologies

* **Kali Linux**
* **Nmap**
* **Metasploit Framework**
* **Samba / SMB**
* **Linux**
* **VirtualBox**
* **TCP/IP**

---

## Skills Demonstrated

**Reconnaissance**

* Host discovery
* Port scanning
* Service enumeration
* Version detection

**Vulnerability Assessment**

* Service fingerprinting
* Vulnerability identification
* Exploit mapping
* Risk assessment

**Exploitation**

* Metasploit module selection
* Exploit configuration
* Reverse-shell handling
* Exploitation validation

**Post-Exploitation**

* Shell access validation
* Privilege verification
* System-level access assessment

---

## Assessment Result

| Metric             | Result                         |
| ------------------ | ------------------------------ |
| Target Identified  | `192.168.56.102`               |
| Vulnerable Service | Samba                          |
| Version            | `2.2.1a`                       |
| Vulnerability      | `trans2open`                   |
| Exploitation       | Successful                     |
| Shell Access       | Obtained                       |
| Privilege Level    | **root**                       |
| Overall Impact     | **Complete System Compromise** |

---

## Disclaimer

This assessment was performed against an intentionally vulnerable **Kioptrix laboratory environment** for educational and authorized security-testing purposes. The techniques documented in this repository must only be performed against systems for which explicit authorization has been obtained.
