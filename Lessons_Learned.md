# Lessons Learned

## Overview

This lab focused on identifying and investigating Windows lateral movement techniques commonly observed after privilege escalation.

The objective was to understand how attackers move between systems using legitimate Windows administration tools and how SOC analysts detect this activity.

---

# Technical Lessons

## Remote Desktop Protocol

Remote Desktop generates multiple Windows Security Events that can identify successful logons, failed logons, session reconnects, and disconnects.

Always investigate:

- Source IP
- Destination Host
- User Account
- Logon Time

---

## PsExec

PsExec creates new processes on remote systems.

Monitor:

- Parent Process
- Command Line
- User
- Host

---

## SMB Administrative Shares

Administrative shares are frequently abused for lateral movement.

Monitor:

- Share Name
- Source Address
- Destination Host
- User

---

## PowerShell Remoting

PowerShell Remoting provides legitimate administration capabilities but is also widely abused.

Review:

- CommandLine
- ParentImage
- User
- Target Host

---

## WinRM

WinRM provides remote PowerShell management.

Unexpected WinRM sessions should always be investigated.

---

# Investigation Skills Improved

✔ Windows Security Event Analysis

✔ Sysmon Investigation

✔ Network Log Analysis

✔ Threat Hunting

✔ Detection Engineering

✔ Timeline Analysis

✔ MITRE ATT&CK Mapping

✔ Incident Documentation

---

# SOC Analyst Mindset

Every lateral movement investigation should answer:

Who initiated the connection?

Where did it originate?

Which destination host?

Which remote administration tool was used?

Was the activity authorized?

Does it match attacker behavior?

Can the attacker move to additional systems?

Should the incident be escalated?

---

# Detection Improvements

Enable:

PowerShell Logging

WinRM Logging

Sysmon

Windows Security Auditing

SMB Auditing

Network Monitoring

Deploy EDR

---

# Future Improvements

Deploy Wazuh

Create Sigma Rules

Build Splunk Dashboards

Investigate WMI Lateral Movement

Investigate DCOM

Investigate Remote Service Creation

Test Atomic Red Team Lateral Movement

---

# Final Reflection

This lab significantly improved my understanding of Windows lateral movement techniques.

By combining Windows Security Event Logs, Sysmon telemetry, and Splunk investigations, I developed practical experience detecting remote authentication, remote administration, SMB activity, and PowerShell remoting.

The investigation methodology closely resembles real-world SOC operations and strengthened my incident response and threat hunting skills.
