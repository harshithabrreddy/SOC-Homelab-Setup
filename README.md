# SOC Mini Homelab Project

## Overview

This project demonstrates a practical Security Operations Center (SOC) mini lab where multiple systems send security logs to a centralized **[Wazuh / Splunk / Elastic]** SIEM server for monitoring and analysis.

The goal of this lab is to understand how SOC teams collect logs from different machines, forward them to a centralized SIEM platform, and analyze them to monitor security events and detect suspicious activity.

---

## Lab Environment

The SOC lab consists of the following systems connected within the same isolated network.

### Attacker Machine
- **Operating System:** Kali Linux
- **Purpose:** Simulate attacker activities (port scans, brute force, malicious PowerShell, etc.) to generate security events within the lab.

### Device 1 – Endpoint System
- **Operating System:** Windows 11
- **Role:** Endpoint machine sending logs to the SIEM
- **Configuration:**
  - [Wazuh Agent / Splunk Universal Forwarder] installed
  - Sysmon installed (SwiftOnSecurity config)
  - Windows Event Logs and PowerShell logs forwarded to the SIEM server

### Device 2 – Server System
- **Operating System:** Windows Server
- **Role:** Server machine sending logs to the SIEM
- **Configuration:**
  - [Wazuh Agent / Splunk Universal Forwarder] installed
  - System and Security logs forwarded to the SIEM

### Device 3 – SIEM Server
- **Operating System:** Ubuntu Server
- **Role:** Centralized logging and monitoring system
- **Configuration:**
  - [Wazuh Manager / Splunk Enterprise / Elastic Stack] installed
  - Receives logs from Windows 11 and Windows Server
  - Used for log analysis, alerting, and monitoring

---

## Architecture

![Architecture Diagram](architecture.png)

### Architecture Explanation
The SOC lab consists of multiple machines connected within the same isolated virtual network.

**Attacker Machine**
- Kali Linux is used to simulate attacker activity (e.g., Nmap scans, SSH/RDP brute force, Atomic Red Team techniques).

**Endpoint Devices**
- Windows 11
- Windows Server
- Ubuntu Server

These systems generate system, security, and application logs.

**Log Collection**
Each endpoint forwards logs using an agent/forwarder to the central SIEM server.

**SIEM Server**
The SIEM platform collects, indexes, and centralizes logs from all machines for monitoring, detection, and analysis.

---

## Tools Used

| Tool | Purpose |
|---|---|
| [Wazuh / Splunk Enterprise] | SIEM platform for centralized log monitoring and analysis |
| [Wazuh Agent / Splunk Universal Forwarder] | Forwards logs from endpoints to the SIEM server |
| Sysmon | Enhanced Windows event logging (process creation, network, etc.) |
| Kali Linux | Attacker machine used to simulate security events |
| Windows 11 | Endpoint system generating logs |
| Windows Server | Server system generating logs |
| Ubuntu Server | Host system running the SIEM |

---

## Log Forwarding Setup

The agent/forwarder was installed on:
- Windows 11
- Windows Server

Both machines were configured to forward logs to the SIEM server running on Ubuntu.

**Forwarded Logs**
- Windows Event Logs
- Security Logs
- System Logs
- Sysmon Logs (process creation, network connections)

Logs are collected and indexed in the SIEM for monitoring and analysis.

---

## Attack Simulations & Detections

| # | Simulated Attack | MITRE ATT&CK ID | Detected? |
|---|---|---|---|
| 1 | SSH/RDP brute force | T1110 | ✅ |
| 2 | Port scanning (Nmap) | T1046 | ✅ |
| 3 | Suspicious PowerShell / encoded commands | T1059.001 | ✅ |
| 4 | New user / privilege escalation | T1136 / T1068 | ✅ |

Full incident reports are in the [`incident-reports/`](incident-reports/) folder.

---

## Monitoring

Using the SIEM web interface, logs from all machines are centralized and monitored. The SIEM is used to:
- View and correlate security events
- Analyze system and process activity
- Investigate login attempts and failed authentications
- Monitor system logs from multiple machines in one dashboard

---

## Screenshots

Screenshots of dashboards, log searches, and agent/forwarder status are in [`screenshots/`](screenshots/):
- SIEM Dashboard
- Log Search Results
- Agent/Forwarder Status
- Sample Detection Alert

---

## Skills Demonstrated

- SOC lab design and setup
- SIEM deployment and configuration
- Log forwarding / agent configuration
- Security log analysis and correlation
- Endpoint monitoring (Sysmon, Windows Event Logs)
- Basic attack simulation and detection engineering
- MITRE ATT&CK mapping
- Incident documentation

---

## Lab Setup Guide

To recreate this SOC lab environment, follow the setup guide:
➡️ [View Lab Setup Guide](LAB_SETUP.md)

---

## Learning Outcome

This project helped in understanding how a SOC environment collects and analyzes logs from multiple systems using a SIEM platform. It demonstrates the practical setup of centralized logging, log forwarding, detection engineering, and monitoring — core skills used by SOC Analysts (Tier 1/2) in real environments.
