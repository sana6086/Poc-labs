# Network Traffic Sniffing & Analysis

> **Network Security | ARP Spoofing | Man-in-the-Middle Analysis | Wireshark**

## Executive Summary

This assessment focused on diagnosing network instability affecting secure web communications.

A controlled **Man-in-the-Middle (MITM)** position was established using ARP poisoning between a target workstation and the network gateway. Network traffic was subsequently captured and analyzed using **Wireshark** to identify communication anomalies.

The analysis identified **packet loss, missing TCP segments, and repeated TCP retransmissions**, indicating network-level instability. Potential contributing factors included network congestion, hardware limitations, and wireless signal interference.

## Assessment Objectives

* Establish a controlled MITM position for network traffic analysis
* Capture and inspect live network traffic
* Identify packet delivery and transport-layer anomalies
* Analyze TLS-based web communication
* Determine potential causes of network instability
* Provide remediation recommendations

## Environment

| Component              | Details          |
| ---------------------- | ---------------- |
| Operating System       | Kali Linux       |
| Interception Technique | ARP Poisoning    |
| Analysis Tool          | Wireshark        |
| Traffic Tool           | `arpspoof`       |
| Target Application     | OWASP Juice Shop |
| Target Workstation     | `192.168.18.73`  |
| Network Gateway        | `192.168.18.1`   |
| Observed Protocol      | TLS 1.2 / TCP    |

## Assessment Methodology

### 01 — Traffic Interception

The diagnostic system was positioned between the target workstation and network gateway through bidirectional ARP poisoning.

The target workstation was configured to associate the diagnostic machine with the gateway, while the gateway was simultaneously configured to associate the diagnostic machine with the target workstation. This enabled the diagnostic system to observe traffic traversing the network path.

### 02 — Traffic Identification

Following traffic interception, **Wireshark** was used to isolate traffic associated with the OWASP Juice Shop application.

**Display filter:**

```text
frame contains "juice"
```

The capture revealed a **TLS 1.2 Client Hello** originating from the target workstation toward the Juice Shop server, confirming the relevant secure web session.

### 03 — Network Analysis

The captured traffic was examined for packet delivery issues and transport-layer anomalies.

The analysis identified:

```text
Previous segment not captured
```

along with multiple:

```text
TCP Retransmission
```

events.

## Key Findings

| Finding               | Observation                     | Assessment                                        |
| --------------------- | ------------------------------- | ------------------------------------------------- |
| Packet Loss           | `Previous segment not captured` | Missing packets observed in the capture           |
| TCP Retransmissions   | Multiple retransmission events  | Data required repeated transmission               |
| Network Instability   | Repeated delivery issues        | Indicates unreliable network communication        |
| Secure Traffic Impact | TLS traffic affected            | Secure web communications experienced instability |

## Root Cause Assessment

The observed packet-loss and retransmission behavior was consistent with network instability.

Potential contributing factors identified in the assessment include:

* Network congestion
* Network hardware limitations
* Wireless signal interference

These conditions can negatively affect reliable delivery of encrypted network traffic.

## Recommendations

### 1. Transition Critical Systems to Wired Connectivity

Deploy shielded Ethernet connections for critical workstations where practical to reduce potential wireless interference and improve connection reliability.

### 2. Evaluate Network Hardware Capacity

Review the capacity and performance of the existing router/switch infrastructure and consider higher-throughput enterprise hardware where required.

### 3. Implement Quality of Service

Configure **QoS policies** to prioritize business-critical HTTPS/TLS traffic over non-essential background traffic.

## Tools & Technologies

* **Kali Linux**
* **arpspoof**
* **Wireshark**
* **OWASP Juice Shop**
* **TCP/IP**
* **ARP**
* **TLS 1.2**

## Skills Demonstrated

* Network traffic interception
* ARP poisoning analysis
* MITM assessment methodology
* Packet capture and analysis
* TCP troubleshooting
* Network anomaly identification
* Wireshark filtering and investigation
* Root-cause analysis
* Security assessment reporting

## Disclaimer

This assessment was conducted in a controlled and authorized environment for educational and security-analysis purposes. Network interception and ARP spoofing techniques must only be performed on systems and networks for which explicit authorization has been obtained.
