# 🛡️ SOC Lab 05 – Windows Lateral Movement Investigation using Splunk & Sysmon

---

# 📖 Project Overview

Lateral Movement is one of the most critical stages of a cyber attack. After obtaining access to one system, attackers attempt to move to additional hosts to expand control, access sensitive information, and compromise critical infrastructure.

This project demonstrates how common Windows lateral movement techniques generate telemetry that can be investigated using Splunk Enterprise and Microsoft Sysmon.

The lab safely simulates remote administrative activity using native Windows tools and generates endpoint telemetry for SOC investigation.

---

# 🎯 Objectives

This lab focuses on investigating common Windows lateral movement techniques.

- Remote Desktop Protocol (RDP)
- PsExec
- PowerShell Remoting
- SMB Administrative Shares
- WinRM
- Network Connections
- Threat Hunting
- Detection Engineering
- MITRE ATT&CK Mapping
- Incident Documentation

---

# 🏢 SOC Scenario

Company

ABC Bank

Role

SOC Analyst L1

Priority

CRITICAL

SOC Ticket

SOC-0005

Alert

Potential Lateral Movement Activity Detected.

A workstation initiated remote administrative activity toward another endpoint inside the corporate network.

Investigate immediately.

---

# 🖥️ Lab Environment

| Component | Technology |
|------------|------------|
| Operating System | Windows 11 |
| SIEM | Splunk Enterprise |
| Endpoint Monitoring | Sysmon |
| Windows Logs | Security Event Logs |
| Remote Access | RDP |
| Remote Administration | PsExec |
| Remote Shell | PowerShell Remoting |
| File Sharing | SMB |
| Remote Management | WinRM |

---

# 📚 Skills Demonstrated

- Windows Lateral Movement Investigation
- Splunk SPL
- Windows Security Log Analysis
- Sysmon Investigation
- Threat Hunting
- Detection Engineering
- Network Log Analysis
- Process Creation Investigation
- Remote Administration Monitoring
- MITRE ATT&CK Mapping
- SOC Documentation

---

# 🛠️ Techniques Covered

## 1. Remote Desktop Protocol (RDP)

Windows Events

4624

4625

4778

4779

MITRE

T1021.001

---

## 2. PsExec

Sysmon

Event ID 1

Windows

4688

MITRE

T1021.002

---

## 3. SMB Administrative Shares

Windows Event

5140

MITRE

T1021.002

---

## 4. PowerShell Remoting

Sysmon Event

1

MITRE

T1059.001

---

## 5. WinRM

Windows Remote Management Logs

MITRE

T1021

---

# 🔍 Investigation Workflow

```
Initial Access

↓

Privilege Escalation

↓

Lateral Movement

↓

Windows Security Logs

↓

Sysmon

↓

Splunk Enterprise

↓

Threat Hunting

↓

Detection Engineering

↓

Incident Report

↓

MITRE ATT&CK Mapping
```

---

# 📁 Repository Structure

```
SOC-Lab-05-Lateral-Movement/

README.md

Attack-Simulation.md

Investigation.md

Detection.md

MITRE.md

Incident_Report.md

Lessons_Learned.md

Queries/

Logs/
```

---

# 📂 Queries Included

- RDP Investigation
- PsExec Investigation
- SMB Share Investigation
- PowerShell Remoting Investigation
- Network Connection Analysis
- Detection Rules
- Timeline Analysis

---

# 🧠 Learning Outcomes

After completing this lab I learned:

- How attackers move laterally across Windows environments
- How RDP generates Windows Security events
- How PsExec appears in Sysmon logs
- How SMB administrative shares are monitored
- How PowerShell Remoting is detected
- How to perform lateral movement threat hunting
- How to create Splunk detections
- How to document investigations using SOC methodology

---

# 🛡️ MITRE ATT&CK Mapping

| Technique | ID |
|------------|-------------|
| Remote Desktop Protocol | T1021.001 |
| SMB / Windows Admin Shares | T1021.002 |
| Remote Services | T1021 |
| PowerShell | T1059.001 |

---

# 📌 Future Improvements

- Deploy Microsoft Defender for Endpoint
- Integrate Wazuh
- Build Sigma Rules
- Simulate PsExec using Atomic Red Team
- Create Splunk Dashboards
- Investigate WMI Lateral Movement
- Monitor Remote Service Creation

---

# 👨‍💻 Author

Gampanapalli Madhu

Cybersecurity Enthusiast
