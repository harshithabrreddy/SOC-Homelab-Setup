# SOC Mini Homelab – Setup Guide

This guide walks through building the SOC mini homelab from scratch.

## Prerequisites

- Host machine: 16 GB RAM minimum (32 GB recommended), 250+ GB free disk space
- Virtualization software: VirtualBox, VMware Workstation/Player, or Proxmox
- ISOs: Kali Linux, Windows 11, Windows Server 2019/2022, Ubuntu Server 22.04
- SIEM installer: [Wazuh OVA/install script / Splunk Enterprise installer]

---

## Step 1: Network Setup

1. Create an **isolated internal/host-only network** in your hypervisor (e.g., `SOC-Lab-Net`).
2. Assign static IPs to each VM on this network, for example:

   | Machine | IP Address |
   |---|---|
   | Ubuntu Server (SIEM) | 192.168.56.10 |
   | Windows 11 (Endpoint) | 192.168.56.20 |
   | Windows Server | 192.168.56.30 |
   | Kali Linux (Attacker) | 192.168.56.40 |

3. Optionally add a second NAT-only adapter on each VM for internet access (updates only) — keep this separate from the lab network.

---

## Step 2: Build the SIEM Server (Ubuntu Server)

1. Install Ubuntu Server 22.04 as a VM.
2. Update the system:
   ```
   sudo apt update && sudo apt upgrade -y
   ```
3. Install the SIEM platform:
   - **Wazuh (recommended, free):** run the official all-in-one install script from the Wazuh documentation.
   - **Splunk Enterprise:** download the `.deb` package and install with `dpkg -i`.
4. Verify the web interface is reachable at `https://<SIEM-IP>:<port>`.
5. Note down the deployment/forwarding credentials or certificates needed for agents.

---

## Step 3: Configure Windows 11 Endpoint

1. Install Windows 11 as a VM, join it to the lab network.
2. Install **Sysmon** using the SwiftOnSecurity config for richer telemetry:
   ```
   Sysmon64.exe -accepteula -i sysmonconfig-export.xml
   ```
3. Enable PowerShell Script Block Logging via Group Policy or registry.
4. Install the **Wazuh Agent** (or **Splunk Universal Forwarder**):
   - Configure it to point to the SIEM server IP.
   - Confirm the agent shows as "Active"/"Connected" on the SIEM dashboard.

---

## Step 4: Configure Windows Server

1. Install Windows Server, join it to the lab network.
2. Enable auditing for logon events, account management, and privilege use (via Local Security Policy → Advanced Audit Policy).
3. Install the agent/forwarder and point it to the SIEM server.
4. Verify logs (System, Security) are being received on the SIEM.

---

## Step 5: Configure Kali Linux (Attacker)

1. Install Kali Linux as a VM on the same lab network.
2. Confirm it can reach the Windows endpoints and Windows Server (ping, port scan).
3. No agent needed — this machine generates attack traffic only.

---

## Step 6: Validate Log Flow

On the SIEM web interface, confirm you can see live/recent events from all three log sources:
- Windows 11 (endpoint)
- Windows Server
- (Ubuntu Server itself, if self-monitoring is enabled)

Take a screenshot of the agent/forwarder status page for the repo.

---

## Step 7: Run Attack Simulations

From Kali, run a few basic simulations and confirm they generate matching alerts in the SIEM:

| Simulation | Command/Tool | Expected Detection |
|---|---|---|
| Port scan | `nmap -sS <target-IP>` | Multiple connection attempts / port scan alert |
| SSH brute force | `hydra -l admin -P wordlist.txt ssh://<target-IP>` | Repeated failed logon events |
| RDP brute force | `hydra -l administrator -P wordlist.txt rdp://<target-IP>` | Windows Event ID 4625 (failed logon) spikes |
| Suspicious PowerShell | Run an encoded command (`powershell -enc ...`) on the Windows VM | PowerShell script block log entry |

For each simulation, capture:
- The attack command/output on Kali
- The matching event(s)/alert in the SIEM
- A short write-up in `incident-reports/`

---

## Step 8: Build Detections & Dashboards

1. Create custom rules (Wazuh) or saved searches/dashboards (Splunk) for each simulated attack.
2. Map each rule to its MITRE ATT&CK technique ID.
3. Save queries/rules into the `detections/` folder of the repo.

---

## Step 9: Document Everything

- Save screenshots into `screenshots/`.
- Write one incident report per scenario in `incident-reports/`, using this structure:
  - Summary
  - Timeline
  - Affected assets
  - Evidence (log snippets/screenshots)
  - MITRE ATT&CK mapping
  - Root cause
  - Containment & remediation
  - Recommendations

---

## Troubleshooting Tips

- If an agent shows "disconnected," check firewall rules between the endpoint and SIEM server (allow the agent's port, e.g., 1514/1515 for Wazuh, 9997 for Splunk forwarders).
- If no logs appear, confirm the correct Windows Event Log channels are enabled and the agent config file points to the right log sources.
- Use `ping` and `telnet <SIEM-IP> <port>` from the endpoint to confirm network reachability before troubleshooting the agent itself.
