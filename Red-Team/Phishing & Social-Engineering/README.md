# Social Engineering & Phishing Simulation Assessment

> **Social Engineering • Credential Harvesting • Phishing Simulation • Account Compromise • Security Awareness**

## Executive Summary

This project documents a controlled **social engineering and credential-harvesting simulation** designed to evaluate the susceptibility of users to phishing-based account compromise.

The assessment simulated an attacker impersonating an internal IT department and directing a target user to a cloned authentication portal. The exercise demonstrated how social engineering, visual website impersonation, and credential harvesting can be combined to obtain authentication credentials.

The simulation successfully captured the test credentials submitted through the phishing page, demonstrating the significant security impact of user-targeted phishing attacks and the importance of phishing-resistant authentication controls.

---

## Assessment Objectives

* Simulate a controlled credential-harvesting attack
* Evaluate user susceptibility to phishing techniques
* Demonstrate website impersonation risks
* Analyze credential interception during form submission
* Assess potential account-compromise impact
* Identify effective preventive and detective controls
* Recommend security improvements for high-value accounts

---

## Assessment Environment

| Component                 | Details                        |
| ------------------------- | ------------------------------ |
| Assessment Platform       | Kali Linux                     |
| Framework                 | Social-Engineer Toolkit (SET)  |
| Attack Type               | Credential Harvesting          |
| Social Engineering Vector | Phishing Email                 |
| Impersonated Service      | Google Account Login           |
| Test Server               | `10.45.225.79`                 |
| Web Protocol              | HTTP                           |
| Target                    | Controlled Test User           |
| Assessment Type           | Authorized Security Simulation |

SET was used to clone the authentication interface, host the simulated phishing page, and capture submitted form data within the controlled environment.

---

## Attack Chain

```text
Social Engineering Pretext
          │
          ▼
     Phishing Lure
          │
          ▼
   Malicious Login Link
          │
          ▼
   Cloned Authentication
          │
          ▼
    User Interaction
          │
          ▼
 Credential Submission
          │
          ▼
 Credential Interception
          │
          ▼
   Account Compromise
```

---

# Assessment Methodology

## 01 — Environment Preparation

A Kali Linux environment containing the **Social-Engineer Toolkit (SET)** was prepared for the authorized simulation.

SET's credential-harvesting functionality was used to establish a controlled phishing environment.

---

## 02 — Authentication Portal Simulation

The legitimate Google login interface was cloned and hosted on the controlled assessment system.

The simulated authentication page was made available through the assessment server:

```text
http://10.45.225.79
```

The cloned page was designed to demonstrate how closely impersonated authentication interfaces can resemble legitimate login portals.

---

## 03 — Phishing Simulation

A simulated phishing email was constructed using an **internal IT support impersonation** scenario.

The lure incorporated common social engineering indicators:

* Spoofed sender identity
* Urgency-based account-security pretext
* Request for immediate verification
* Link directing the user to the simulated login portal

The phishing link directed the test user to the controlled assessment server.

---

## 04 — User Interaction

The test user followed the simulated phishing link and was presented with a cloned authentication interface.

Although the page closely resembled the legitimate login experience, the environment exposed indicators such as:

* HTTP rather than HTTPS
* An IP address rather than the legitimate domain
* Browser security warnings

The assessment demonstrated that users who do not validate the destination URL and browser security indicators may be susceptible to visually convincing phishing pages.

---

## 05 — Credential Capture

When the test user submitted the authentication form, the request was processed by the credential-harvesting component of SET.

The assessment successfully captured the submitted test username and password in plaintext within the controlled environment.

This confirmed the effectiveness of the simulated phishing workflow against the test account.

---

# Security Findings

## Finding 01 — Credential Harvesting Through Phishing

**Severity:** Critical
**Category:** Social Engineering / Credential Theft

### Description

The simulation demonstrated that a user can be deceived into submitting credentials to an attacker-controlled authentication interface when presented with a convincing phishing scenario.

### Impact

Successful credential harvesting could enable:

* Unauthorized account access
* Exposure of email and cloud data
* Access to integrated services
* Further credential compromise
* Potential lateral movement

The report assesses the overall scenario as **Critical** because successful compromise of a high-value account can provide access to multiple interconnected services.

---

## Finding 02 — Phishing-Resistant MFA Gap

**Severity:** Critical
**Category:** Authentication Security

### Description

Password-based authentication alone may not adequately protect accounts against credential-phishing attacks.

The assessment recommends phishing-resistant authentication mechanisms such as **FIDO2/WebAuthn security keys**, which validate the legitimate authentication domain before allowing authentication.

### Recommended Control

Deploy phishing-resistant MFA for:

* Privileged accounts
* Executive accounts
* Administrative accounts
* High-value corporate identities

---

## Finding 03 — Session Security Exposure

**Severity:** High
**Category:** Session Management

The assessment identified a persistent-session parameter within the captured form submission.

The report highlights the possibility of session abuse when persistent authentication mechanisms are compromised.

### Recommended Control

Implement:

* Shorter session lifetimes
* Secure session-cookie configuration
* Appropriate session invalidation
* Additional risk-based authentication controls

---

# Business Impact

A successful phishing attack against a privileged or executive identity could result in significant organizational exposure.

Potential consequences include:

### Account Compromise

Unauthorized access to corporate email and connected cloud services.

### Data Exposure

Potential disclosure of confidential communications, documents, financial information, and organizational data.

### Lateral Movement

Compromised identity credentials may provide access to connected SSO applications, internal resources, and cloud infrastructure.

### Operational & Reputational Impact

Compromised executive or administrative accounts can facilitate additional attacks and create significant organizational and reputational consequences.

---

# Recommendations

## 1. Deploy Phishing-Resistant MFA

Implement **FIDO2/WebAuthn** authentication for privileged, executive, and other high-value accounts.

This provides stronger protection against credential-phishing attacks than password-based authentication alone.

## 2. Strengthen Email Security

Deploy email-security controls incorporating:

* URL inspection
* Domain-spoofing detection
* DMARC
* DKIM
* SPF

These controls can reduce the likelihood of phishing messages reaching users.

## 3. Enforce HTTPS

Implement HTTPS-only browser policies and prevent users from submitting credentials through insecure HTTP connections.

Users should also be trained to verify:

* Domain names
* HTTPS indicators
* Browser security warnings
* Unexpected authentication requests

## 4. Improve Session Management

Use appropriate session timeouts and session-management controls to reduce the potential impact of compromised authentication sessions.

## 5. Conduct Security Awareness Training

Regular phishing simulations should be conducted to train employees to identify:

* Urgency-based requests
* Suspicious sender addresses
* Unexpected login requests
* Suspicious URLs
* Fake authentication pages
* Insecure HTTP login pages

---

# Tools & Technologies

| Tool / Technology                 | Purpose                             |
| --------------------------------- | ----------------------------------- |
| **Kali Linux**                    | Security testing platform           |
| **Social-Engineer Toolkit (SET)** | Social engineering simulation       |
| **Credential Harvester**          | Controlled credential capture       |
| **HTTP**                          | Simulated phishing endpoint         |
| **Google Login Clone**            | Authentication-interface simulation |

---

# Skills Demonstrated

### Social Engineering

* Phishing simulation
* Pretext development
* User-awareness assessment
* Credential-harvesting analysis

### Offensive Security

* SET configuration
* Authentication-page simulation
* Form-submission interception
* Attack-chain analysis

### Defensive Security

* Phishing risk assessment
* Authentication control evaluation
* Session-security analysis
* Security-awareness recommendations
* Phishing-resistant MFA evaluation

---

# Assessment Results

| Assessment Area                | Result           |
| ------------------------------ | ---------------- |
| Phishing Simulation            | ✅ Successful     |
| Authentication Page Simulation | ✅ Successful     |
| User Interaction               | ✅ Successful     |
| Credential Capture             | ✅ Successful     |
| Test Account Exposure          | Demonstrated     |
| Primary Risk                   | Credential Theft |
| Overall Severity               | **Critical**     |

---

# Key Takeaway

This assessment demonstrates that **human interaction remains a significant attack surface even when technically secure authentication infrastructure is in place**.

Effective defense requires a combination of **phishing-resistant MFA, secure email controls, strong session management, HTTPS enforcement, and continuous security-awareness training**.

---

## Disclaimer

This assessment was conducted exclusively as an authorized security simulation using controlled test infrastructure and test credentials. No real user credentials or unauthorized accounts were targeted. Social engineering, phishing, and credential-harvesting techniques must only be performed with explicit authorization and within an approved assessment scope.
