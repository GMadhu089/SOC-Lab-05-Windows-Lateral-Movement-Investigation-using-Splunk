# MITRE ATT&CK Mapping

## Overview

MITRE ATT&CK is a globally recognized knowledge base that documents adversary tactics and techniques observed in real-world cyber attacks.

This lab focuses on the **Lateral Movement** tactic, where attackers move from one compromised host to another using legitimate administrative tools and protocols.

Understanding these techniques helps SOC analysts create effective detections and conduct accurate investigations.

---

# MITRE Tactic

## TA0008 – Lateral Movement

Lateral Movement consists of techniques used by attackers to access and control additional systems within a network after the initial compromise.

Objectives include:

- Expanding access
- Reaching critical servers
- Accessing sensitive data
- Preparing for persistence or exfiltration

---

# Technique 1

## Remote Desktop Protocol (RDP)

### Technique ID

T1021.001

### Description

Attackers use Remote Desktop Protocol to remotely log into another Windows system using valid credentials.

---

### Why Attackers Use RDP

- Native Windows feature
- Interactive desktop access
- Administrative control
- Easy movement between systems

---

### Windows Events

4624

4625

4778

4779

---

### Investigation Questions

Who initiated the RDP session?

Which source IP?

Which destination host?

Was the login successful?

Was the activity expected?

---

# Technique 2

## SMB / Windows Administrative Shares

### Technique ID

T1021.002

---

### Description

Attackers access hidden administrative shares such as:

ADMIN$

C$

IPC$

to copy files, execute tools, or stage payloads.

---

### Windows Event

5140

---

### Investigation Questions

Which share was accessed?

Who accessed it?

Which source IP?

Was the access authorized?

---

# Technique 3

## PsExec

### Technique ID

T1021.002

---

### Description

PsExec allows administrators to remotely execute commands.

Attackers abuse PsExec for:

- Remote command execution
- Malware deployment
- Lateral movement

---

### Logs

Sysmon Event 1

Windows Event 4688

---

### Investigation Questions

Who executed PsExec?

Which process launched it?

Which remote host?

---

# Technique 4

## Windows Remote Management (WinRM)

### Technique ID

T1021

---

### Description

WinRM enables remote management using PowerShell.

Commonly used with:

Enter-PSSession

Invoke-Command

---

### Investigation Questions

Which account initiated WinRM?

Which host accepted the connection?

Was the activity expected?

---

# Technique 5

## PowerShell

### Technique ID

T1059.001

---

### Description

PowerShell is frequently used for remote administration and post-exploitation.

Attackers may use it to:

- Execute remote commands
- Download payloads
- Move laterally
- Manage remote systems

---

### Investigation Questions

Was PowerShell launched remotely?

Was it encoded?

Was it elevated?

Did it connect to another endpoint?

---

# MITRE Summary

| Technique | ID |
|-----------|------|
| Remote Desktop Protocol | T1021.001 |
| SMB / Windows Admin Shares | T1021.002 |
| Remote Services | T1021 |
| PowerShell | T1059.001 |

---

# Defensive Recommendations

✔ Enable Sysmon

✔ Enable Windows Security Auditing

✔ Monitor RDP Logons

✔ Restrict SMB Administrative Shares

✔ Enable PowerShell Logging

✔ Enable WinRM Logging

✔ Review Network Connections

✔ Deploy EDR

✔ Perform Continuous Threat Hunting

---

# SOC Analyst Takeaway

Lateral movement rarely consists of a single event.

Successful investigations require correlating authentication logs, process creation, network connections, and remote administration activity to reconstruct the attack path across multiple systems.
