# SOC Mini Homelab Project

## Overview

This project demonstrates a practical Security Operations Center (SOC) mini lab where multiple systems send security logs to a centralized **[Wazuh / Splunk / Elastic]** SIEM server for monitoring and analysis.

The goal of this lab is to understand how SOC teams collect logs from different machines, forward them to a centralized SIEM platform, and analyze them to monitor security events and detect suspicious activity.

---

## Lab Environment

The SOC lab consists of the following systems connected within the same isolated network.

### Attacker Machine
- **Operating System:** Kali Linux
- <img width="1156" height="534" alt="WhatsApp Image 2026-09-23 at 15 58 54" src="https://github.com/user-attachments/assets/a8a460e3-decf-4a6a-9a27-5460b9c67107" />

- **Purpose:** Simulate attacker activities (port scans, brute force, malicious PowerShell, etc.) to generate security events within the lab.

### Device 1 – Endpoint System
- **Operating System:** Windows 11
- <img width="1156" height="534" alt="image" src="https://github.com/user-attachments/assets/aa1ebbc8-3aac-43cb-9499-2d8fc0d7ec22" />

- **Role:** Endpoint machine sending logs to the SIEM
- **Configuration:**
  - [Wazuh Agent / Splunk Universal Forwarder] installed
  - Sysmon installed (SwiftOnSecurity config)
  - Windows Event Logs and PowerShell logs forwarded to the SIEM server

### Device 2 – Server System
- **Operating System:** Windows Server
- <img width="1156" height="534" alt="image" src="https://github.com/user-attachments/assets/c2093531-aa26-4d2a-926c-8e7c8627c9b5" />

- **Role:** Server machine sending logs to the SIEM
- **Configuration:**
  - [Wazuh Agent / Splunk Universal Forwarder] installed
  - System and Security logs forwarded to the SIEM

### Device 3 – SIEM Server
- **Operating System:** Ubuntu Server
- <img width="1920" height="1080" alt="Screenshot (491)" src="https://github.com/user-attachments/assets/efe78a39-2294-4a14-8d81-fd0ada430fd0" />
- <img width="1920" height="1080" alt="Screenshot (252)" src="https://github.com/user-attachments/assets/8110cc1b-ede5-4c06-bfcc-7c80fea50ddc" />
SPL Quires Practiced
<img width="1920" height="1080" alt="Screenshot (225)" src="https://github.com/user-attachments/assets/b76e4007-bdfc-4aa0-8dc8-8aab58ae0078" />



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
