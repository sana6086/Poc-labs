# Temple of Doom — Penetration Testing Assessment

> **Web Application Security | Remote Code Execution | Post-Exploitation | Privilege Escalation | Linux Security**

## Executive Summary

This assessment documents a controlled penetration test against the **Temple of Doom** vulnerable laboratory environment.

The engagement evaluated the target from an external attacker's perspective, progressing through reconnaissance, service enumeration, application analysis, vulnerability identification, exploitation, post-exploitation, and privilege escalation.

The assessment identified an **insecure deserialization vulnerability** within the Node.js application. Client-controlled serialized data was processed through an unsafe `unserialize()` function, enabling remote code execution and providing an initial shell as the `nodeadmin` user.

## Post-exploitation analysis identified an accessible `ss-manager` service, which enabled privilege transition to the `fireman` account. Further analysis of `sudo` permissions revealed unrestricted passwordless execution of privileged utilities, including `tcpdump`. This weakness was leveraged to obtain **root-level access**, demonstrating complete compromise of the target system.

## Assessment Objectives

The assessment was conducted to:

* Identify externally accessible network services
* Enumerate the target's application and technology stack
* Analyze application behavior and exposed information
* Identify exploitable vulnerabilities
* Validate remote code execution
* Establish controlled post-exploitation access
* Assess local privilege boundaries
* Validate privilege escalation
* Determine the overall security impact
* Provide remediation recommendations

---

## Target Environment

| Attribute                    | Value                    |
| ---------------------------- | ------------------------ |
| Target                       | Temple of Doom           |
| Target IP                    | `192.168.56.108`         |
| Attacker Platform            | Kali Linux               |
| Network                      | `192.168.56.0/24`        |
| Exposed Web Service          | `666/tcp`                |
| Remote Administration        | `22/tcp` / SSH           |
| Application Stack            | Node.js / Express        |
| Initial Access               | Insecure Deserialization |
| Initial Account              | `nodeadmin`              |
| Privilege-Escalation Account | `fireman`                |
| Final Privilege              | **root**                 |

---

# Attack Chain

```text
External Reconnaissance
        │
        ▼
Service Enumeration
        │
        ▼
Node.js / Express Application
        │
        ▼
Stack Trace Analysis
        │
        ▼
Insecure Deserialization
        │
        ▼
Remote Code Execution
        │
        ▼
nodeadmin
        │
        ▼
Local Service Enumeration
        │
        ▼
ss-manager
        │
        ▼
fireman
        │
        ▼
Sudo Privilege Analysis
        │
        ▼
tcpdump Abuse
        │
        ▼
ROOT
```

---

# Methodology

The assessment followed a structured penetration-testing workflow:

1. **Reconnaissance**
2. **Service Enumeration**
3. **Application Analysis**
4. **Vulnerability Identification**
5. **Exploitation**
6. **Post-Exploitation**
7. **Privilege Escalation**
8. **Impact Assessment**
9. **Remediation Planning**

---

# 01 — Reconnaissance & Service Enumeration

Initial network reconnaissance identified the target host:

```text
192.168.56.108
```

A full TCP scan identified two exposed services:

|      Port | Service                |
| --------: | ---------------------- |
|  `22/tcp` | SSH                    |
| `666/tcp` | HTTP / Web Application |

Service enumeration identified the application on port `666` as a **Node.js Express application**, establishing the primary attack surface.

---

# 02 — Application Analysis

Initial interaction with the web application returned an **"Under Construction"** response.

Further requests produced a detailed **SyntaxError stack trace**. The exposed trace referenced:

```text
Object.exports.unserialize
```

This was a significant security indicator because it demonstrated that application input was being processed through a deserialization function and that internal implementation details were being disclosed to the client.

---

# Finding F-01 — Insecure Deserialization

**Severity:** Critical
**Category:** CWE-502 — Deserialization of Untrusted Data

### Description

The application processed a client-controlled `profile` cookie through an `unserialize()` function.

The cookie contained serialized data encoded in Base64. Analysis and controlled modification of this data demonstrated that the application trusted serialized client-side input.

### Security Impact

Successful exploitation of insecure deserialization can result in:

* Remote code execution
* Unauthorized command execution
* Application compromise
* Server compromise
* Privilege escalation
* Complete host compromise

### Risk

**Critical**

The vulnerability was successfully exploited during the assessment and resulted in remote command execution on the target.

---

# 03 — Remote Code Execution

Controlled exploitation of the vulnerable deserialization mechanism resulted in execution of attacker-controlled commands on the target system.

A reverse-shell payload was used to establish a controlled command session. The resulting shell executed in the security context of:

```text
nodeadmin
```

### Initial Access

| Attribute     | Result                   |
| ------------- | ------------------------ |
| Vulnerability | Insecure Deserialization |
| Exploitation  | Successful               |
| Execution     | Remote                   |
| Initial User  | `nodeadmin`              |

---

# 04 — Post-Exploitation Analysis

Following initial access, local enumeration was performed to identify application components, configuration files, credentials, services, and potential privilege-escalation paths.

The `nodeadmin` environment contained application-related directories including:

```text
.ssh
.web
.forever
.config
```

Application analysis also identified the Node.js project structure and the `node-serialize` dependency.

---

# 05 — Local Service Enumeration

Process and service enumeration identified an `ss-manager` process operating under the `fireman` account.

Further assessment of the service resulted in a controlled shell as:

```text
fireman
```

This established a second stage in the privilege-escalation path.

### Privilege Transition

```text
nodeadmin
    │
    ▼
ss-manager
    │
    ▼
fireman
```

---

# Finding F-02 — Excessive Sudo Privileges

**Severity:** Critical
**Category:** Privilege Escalation / Excessive Administrative Permissions

### Description

The `fireman` account was permitted to execute the following utilities with root privileges without password authentication:

```text
tcpdump
iptables
nmcli
```

### Security Impact

Granting unrestricted root execution of powerful system utilities can create a direct path to privilege escalation.

An attacker who compromises the `fireman` account could potentially abuse these permissions to execute commands with elevated privileges and compromise the entire operating system.

---

# 06 — Root Privilege Escalation

The permitted `tcpdump` functionality was leveraged during the assessment to execute a controlled command with elevated privileges.

A root shell was successfully established and verified using:

```text
whoami
```

Result:

```text
root
```

This confirmed successful escalation to the highest operating-system privilege level.

---

# Finding F-03 — Information Disclosure Through Stack Traces

**Severity:** Medium
**Category:** Information Disclosure

### Description

The application exposed a detailed error stack trace to the client during malformed input processing.

The trace revealed internal application functionality, including the use of `unserialize()`.

### Security Impact

Detailed error messages can assist attackers by revealing:

* Application internals
* File paths
* Function names
* Framework components
* Vulnerable code paths

This information can significantly reduce the effort required for vulnerability discovery and exploitation.

---

# Findings Summary

| ID   | Finding                         | Severity     | Status     |
| ---- | ------------------------------- | ------------ | ---------- |
| F-01 | Insecure Deserialization → RCE  | **Critical** | Exploited  |
| F-02 | Excessive Sudo Privileges       | **Critical** | Exploited  |
| F-03 | Detailed Stack Trace Disclosure | **Medium**   | Identified |

---

# Business & Security Impact

The identified vulnerabilities formed a complete compromise chain:

```text
Internet-Accessible Application
          ↓
Insecure Deserialization
          ↓
Remote Code Execution
          ↓
nodeadmin
          ↓
Local Service Abuse
          ↓
fireman
          ↓
Excessive Sudo Permissions
          ↓
Root
```

The successful attack demonstrates that vulnerabilities at different layers can be chained together to produce a significantly higher overall impact than any individual finding in isolation.

### Overall Assessment Result

**Complete system compromise was demonstrated.**

---

# Remediation Recommendations

## 1. Remove Unsafe Deserialization

Do not deserialize untrusted client-controlled data using unsafe serialization libraries.

Where structured data is required:

* Prefer safe formats such as JSON
* Validate input against strict schemas
* Avoid dynamic object reconstruction
* Remove vulnerable serialization dependencies
* Maintain current dependency versions

## 2. Protect Security-Sensitive Application State

Do not rely on client-controlled serialized objects for identity, authorization, or other security-sensitive state.

Sensitive state should be maintained server-side and protected through appropriate integrity and access-control mechanisms.

## 3. Restrict Sudo Permissions

Apply the **principle of least privilege**.

Remove unnecessary `NOPASSWD` permissions and restrict administrative commands to the minimum functionality required.

## 4. Secure Local Services

Restrict access to services such as `ss-manager` and ensure administrative services are:

* Not unnecessarily exposed
* Properly authenticated
* Running with the minimum required privileges
* Monitored for suspicious activity

## 5. Disable Detailed Production Errors

Configure the application to return generic error messages to external users while retaining detailed diagnostic information in protected server-side logs.

## 6. Maintain Dependency Security

Regularly audit Node.js dependencies and remove outdated or vulnerable packages such as insecure serialization libraries.

---

# Tools & Technologies

| Tool / Technology     | Primary Use                              |
| --------------------- | ---------------------------------------- |
| **Nmap**              | Host and service enumeration             |
| **Burp Suite**        | HTTP traffic analysis                    |
| **Burp Decoder**      | Data decoding and encoding               |
| **Node.js / Express** | Application analysis                     |
| **Netcat**            | Controlled shell communication           |
| **SSH**               | Post-exploitation access                 |
| **Linux**             | Local enumeration and privilege analysis |

---

# Skills Demonstrated

### Reconnaissance

* Host discovery
* Port scanning
* Service enumeration
* Technology identification

### Web Application Security

* HTTP request analysis
* Cookie manipulation
* Stack-trace analysis
* Insecure deserialization assessment
* Remote code execution validation

### Exploitation

* Payload development
* Controlled RCE validation
* Reverse-shell establishment
* Post-exploitation access

### Privilege Escalation

* Local enumeration
* Service analysis
* Sudo configuration review
* Privileged utility abuse
* Root-access validation

### Reporting

* Attack-path documentation
* Risk classification
* Impact assessment
* Remediation planning

---

# Assessment Outcome

| Metric                 | Result                         |
| ---------------------- | ------------------------------ |
| Target Identified      | `192.168.56.108`               |
| Primary Attack Surface | Node.js / Express              |
| Initial Vulnerability  | Insecure Deserialization       |
| Remote Code Execution  | **Successful**                 |
| Initial Account        | `nodeadmin`                    |
| Intermediate Account   | `fireman`                      |
| Privilege Escalation   | **Successful**                 |
| Final Privilege        | **root**                       |
| Overall Impact         | **Complete System Compromise** |

---

# Key Takeaway

The assessment demonstrates the importance of evaluating vulnerabilities as part of a **complete attack chain** rather than in isolation.

An externally accessible application vulnerability enabled initial code execution, while weaknesses in local service exposure and administrative privileges enabled escalation to root.

Effective remediation therefore requires controls across multiple layers:

**Secure application development → dependency management → least privilege → service hardening → secure configuration → continuous monitoring**

---


## Disclaimer

This assessment was conducted against an intentionally vulnerable laboratory environment for authorized cybersecurity training and educational purposes. The techniques documented in this repository must only be performed against systems for which explicit authorization has been obtained.
