# Detection Engineering

## Objective

Detection Engineering focuses on creating Splunk rules that identify lateral movement techniques while minimizing false positives.

This lab develops detections for Remote Desktop, PsExec, SMB Administrative Shares, PowerShell Remoting, and WinRM.

---

# Detection 1

## Remote Desktop Logon

Windows Event

4624

Logon Type

10

---

## SPL

```spl
index=*
EventCode=4624
LogonType=10
```

Alert Name

Remote Desktop Logon

Severity

High

MITRE

T1021.001

---

### Why Detect It?

Remote Desktop is one of the most commonly abused lateral movement techniques.

Investigate

- User
- Source IP
- Destination Host
- Logon Time

---

### False Positives

- IT Support
- System Administrators
- Remote Employees

---

# Detection 2

## PsExec Execution

Sysmon Event

1

---

## SPL

```spl
index=sysmon
CommandLine="*psexec*"
```

Alert Name

PsExec Execution

Severity

High

MITRE

T1021.002

---

### Investigation

Review

- Parent Process
- Command Line
- User
- Host

---

# Detection 3

## SMB Administrative Share Access

Windows Event

5140

---

## SPL

```spl
index=*
EventCode=5140
```

Alert Name

Administrative Share Access

Severity

Medium

MITRE

T1021.002

---

### Investigate

- Share Name
- User
- Source Address
- Destination Host

---

# Detection 4

## PowerShell Remoting

```spl
index=sysmon
CommandLine="*Enter-PSSession*"
```

Alert Name

PowerShell Remoting Activity

Severity

Medium

MITRE

T1059.001

---

### Investigation

Review

- Parent Process
- User
- Command Line
- Host

---

# Detection 5

## WinRM Activity

```spl
index=*
WinRM
```

Alert Name

WinRM Remote Session

Severity

Medium

MITRE

T1021

---

# Correlation Logic

A high-confidence lateral movement scenario may appear as:

```
Successful Logon (4624)

↓

Network Connection (Sysmon 3)

↓

PowerShell / PsExec Process

↓

SMB Administrative Share Access

↓

Remote Command Execution

↓

Lateral Movement Confirmed
```

---

# Combined Threat Hunting Query

```spl
(
index=* EventCode=4624 LogonType=10
)
OR
(
index=sysmon CommandLine="*psexec*"
)
OR
(
index=* EventCode=5140
)
OR
(
index=sysmon CommandLine="*Enter-PSSession*"
)
```

---

# Dashboard Panels

Recommended dashboard panels

- Remote Desktop Logons
- PsExec Activity
- PowerShell Remoting
- SMB Share Access
- WinRM Sessions
- Network Connections
- Top Source Hosts
- Top Destination Hosts
- Timeline

---

# Detection Tuning

Whitelist

- Domain Administrators
- Helpdesk
- SCCM
- Microsoft Intune
- Backup Servers
- Monitoring Servers

Review

- Source Host
- Destination Host
- User
- Frequency
- Business Hours
- Parent Process

---

# Analyst Recommendations

- Monitor Event IDs 4624, 4688, 5140, 4778, and 4779.
- Deploy Sysmon to all Windows endpoints.
- Restrict RDP access using firewalls.
- Monitor SMB administrative shares.
- Review PowerShell remoting activity.
- Enable WinRM logging.
- Correlate authentication, process creation, and network events.

---

# Final Conclusion

Lateral movement enables attackers to expand control within an environment after initial compromise. By combining Windows Security Event Logs, Sysmon telemetry, and Splunk detections, SOC analysts can identify remote authentication, administrative tools, and remote execution techniques before attackers reach critical systems.
