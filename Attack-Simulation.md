# Attack Simulation

## Objective

The purpose of this lab is to safely generate Windows lateral movement telemetry for investigation using Splunk Enterprise and Microsoft Sysmon.

Only legitimate Windows administrative tools are used.

No malware is executed during this lab.

---

# Attack Overview

Lateral Movement occurs after an attacker gains access to an initial system and attempts to move to additional systems within the network.

Common methods include:

- Remote Desktop Protocol (RDP)
- PsExec
- SMB Administrative Shares
- PowerShell Remoting
- Windows Remote Management (WinRM)

This lab simulates each technique safely to generate logs for analysis.

---

# Attack Flow

```
Compromised Workstation

↓

Remote Authentication

↓

Lateral Movement

↓

Target Endpoint

↓

Administrative Access

↓

Further Compromise
```

---

# Lab 1 – Remote Desktop (RDP)

## Objective

Generate a successful Remote Desktop logon.

---

## Steps

1. Enable Remote Desktop on the target Windows system.
2. From another Windows machine or VM, open **Remote Desktop Connection (mstsc.exe)**.
3. Connect to the target using valid credentials.

---

## Expected Windows Events

| Event ID | Description |
|-----------|-----------------------------|
|4624|Successful Logon|
|4625|Failed Logon|
|4778|Session Reconnected|
|4779|Session Disconnected|

---

## Purpose

Generate Remote Desktop authentication telemetry.

---

# Lab 2 – PowerShell Remoting

## Objective

Generate PowerShell Remoting activity.

---

## Command

```powershell
Enter-PSSession -ComputerName TARGET-PC
```

---

## Expected Logs

Sysmon

Event ID 1

Windows PowerShell Logs

WinRM Logs

---

## Purpose

Generate remote PowerShell telemetry.

---

# Lab 3 – PsExec

## Objective

Generate PsExec process creation events.

---

## Command

```cmd
psexec \\TARGET-PC cmd
```

---

## Expected Logs

| Source | Event |
|----------|----------------|
|Sysmon|Event ID 1|
|Windows|4688 Process Creation|

---

## Purpose

Generate remote execution telemetry.

---

# Lab 4 – SMB Administrative Shares

## Objective

Access an administrative share.

---

## Example

```
\\TARGET-PC\ADMIN$
```

or

```
\\TARGET-PC\C$
```

---

## Expected Windows Event

5140

Network Share Access

---

## Purpose

Generate SMB administrative share telemetry.

---

# Lab 5 – WinRM

## Objective

Generate Windows Remote Management activity.

---

## Example

```powershell
Enter-PSSession -ComputerName TARGET-PC
```

---

## Expected Logs

WinRM Operational Logs

PowerShell Logs

Sysmon Process Creation

---

## Purpose

Generate Windows Remote Management telemetry.

---

# Telemetry Generated

| Source | Event |
|----------|------------------------|
|Windows Security|RDP Logon|
|Windows Security|Network Share Access|
|Windows Security|Process Creation|
|Sysmon|Process Creation|
|Sysmon|Network Connections|
|PowerShell|Remote Session|

---

# Lab Cleanup

Disconnect Remote Desktop sessions.

Close PowerShell Remote Sessions.

Remove temporary PsExec service (if created).

Disable Remote Desktop if it was enabled only for the lab.

Delete temporary test accounts if they were created.

---

# Key Takeaway

This lab demonstrates how attackers move laterally inside Windows environments using legitimate administrative tools.

By analyzing Windows Security Event Logs and Sysmon telemetry, SOC analysts can detect remote authentication, remote process execution, administrative share access, and PowerShell remoting before attackers compromise additional systems.
