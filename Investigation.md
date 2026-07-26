# Windows Lateral Movement Investigation

## Objective

The objective of this investigation is to identify lateral movement activities across Windows systems using Splunk Enterprise, Windows Security Event Logs, and Microsoft Sysmon.

Lateral movement occurs after an attacker gains initial access and attempts to access additional systems using legitimate administrative tools such as Remote Desktop Protocol (RDP), PsExec, SMB administrative shares, Windows Remote Management (WinRM), and PowerShell Remoting.

The investigation answers five critical SOC questions:

- Who initiated the remote activity?
- What remote access technique was used?
- When did it occur?
- Which source and destination systems were involved?
- Was the activity legitimate or suspicious?

---

# Investigation Methodology

## Step 1 – Identify the User

Determine the account initiating the remote connection.

Possible Accounts

- Administrator
- Domain Admin
- Helpdesk
- Service Account
- Standard User

Questions

- Is this account expected to perform remote administration?
- Is the activity occurring during business hours?
- Has this account performed similar actions before?

---

## Step 2 – Identify the Source Host

Determine where the connection originated.

Examples

WIN11-CLIENT

HR-PC

ADMIN-LAPTOP

Questions

- Is this workstation authorized for administration?
- Has it communicated with this destination before?

---

## Step 3 – Identify the Destination Host

Determine the target system.

Examples

FILESERVER01

DC01

SQL01

Questions

- Is the target a critical asset?
- Does the user normally access this host?

---

## Step 4 – Identify the Remote Technique

Possible Techniques

- Remote Desktop (RDP)
- PsExec
- SMB Admin Share
- WinRM
- PowerShell Remoting

---

# Investigation 1 – Remote Desktop (RDP)

Windows Events

4624

4625

4778

4779

---

## SPL Query

```spl
index=*
EventCode=4624
LogonType=10
```

---

## Display Fields

```spl
index=*
EventCode=4624
LogonType=10
| table _time host Account_Name Source_Network_Address LogonType
```

---

### Field Analysis

| Field | Purpose |
|---------|-----------------------------|
| _time | Investigation timeline |
| host | Destination endpoint |
| Account_Name | Logged-on user |
| Source_Network_Address | Source IP Address |
| LogonType | Authentication method |

---

### Questions

Who logged in?

Which IP initiated the connection?

Which endpoint received the connection?

Was RDP expected?

---

# Investigation 2 – PsExec

Expected Logs

Sysmon Event ID 1

Windows Event 4688

---

## SPL Query

```spl
index=sysmon
CommandLine="*psexec*"
```

---

## Display

```spl
index=sysmon
CommandLine="*psexec*"
| table _time User ParentImage Image CommandLine host
```

---

### Questions

Who executed PsExec?

Which parent process launched it?

Which command was executed?

Which endpoint?

---

# Investigation 3 – SMB Administrative Shares

Windows Event

5140

---

## SPL Query

```spl
index=*
EventCode=5140
```

---

## Display

```spl
index=*
EventCode=5140
| table _time Account_Name Share_Name Source_Address host
```

---

### Questions

Who accessed the share?

Which share?

Which source IP?

Was the access expected?

---

# Investigation 4 – PowerShell Remoting

Sysmon Event

1

---

## SPL Query

```spl
index=sysmon
CommandLine="*Enter-PSSession*"
```

---

## Display

```spl
index=sysmon
CommandLine="*Enter-PSSession*"
| table _time User ParentImage CommandLine host
```

---

### Questions

Who initiated PowerShell Remoting?

Which computer?

Was the command legitimate?

---

# Investigation 5 – WinRM

Search

```spl
index=*
WinRM
```

---

### Questions

Which host accepted WinRM?

Which account?

Was it expected?

---

# Network Connection Investigation

Sysmon

Event ID

3

---

## SPL Query

```spl
index=sysmon
EventCode=3
```

---

## Display

```spl
index=sysmon
EventCode=3
| table _time Image SourceIp DestinationIp DestinationPort
```

---

### Questions

Which destination IP?

Which destination port?

Was the connection internal?

---

# Threat Hunting

## Hunt RDP

```spl
index=*
EventCode=4624
LogonType=10
```

---

## Hunt PsExec

```spl
index=sysmon
CommandLine="*psexec*"
```

---

## Hunt SMB

```spl
index=*
EventCode=5140
```

---

## Hunt WinRM

```spl
index=*
WinRM
```

---

## Hunt PowerShell Remoting

```spl
index=sysmon
CommandLine="*Enter-PSSession*"
```

---

# Timeline Analysis

```spl
index=*
(EventCode=4624 OR EventCode=4688 OR EventCode=5140)
| timechart span=5m count by EventCode
```

Timeline helps reconstruct the attack path from source host to destination host.

---

# Analyst Verdict

The observed lateral movement activity was generated in a controlled laboratory using legitimate Windows administrative tools.

The investigation demonstrated that Windows Security Event Logs and Sysmon provide sufficient telemetry to detect remote authentication, remote command execution, SMB administrative share access, PowerShell remoting, and WinRM activity.
