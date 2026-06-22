# 12. WinRM Assessment

## Overview

Windows Remote Management (WinRM) is a Microsoft service that allows remote management of Windows systems using the WS-Management protocol.

It is commonly used in enterprise environments for automation, PowerShell remoting, and system administration. Because WinRM provides remote command execution capabilities, it is a high-value target during internal security assessments.

The objective of this assessment is to identify WinRM exposure, evaluate authentication mechanisms, review access controls, and detect misconfigurations that may enable unauthorized remote execution or lateral movement.

---

## Objectives

* Identify WinRM-enabled systems
* Determine WinRM service exposure
* Assess authentication mechanisms
* Validate remote PowerShell access
* Review user authorization
* Identify misconfigurations
* Assess lateral movement potential
* Document security findings

---

# WinRM Service Identification

## Scan WinRM Ports

```bash id="winrm01"
nmap -p 5985,5986 192.168.1.10
```

Example Output:

```text id="winrm02"
PORT     STATE SERVICE
5985/tcp open  wsman
5986/tcp open  wsmans
```

---

## Service Detection

```bash id="winrm03"
nmap -sV -p 5985,5986 192.168.1.10
```

---

## Check WinRM Configuration

```bash id="winrm04"
nmap --script http-headers -p 5985 192.168.1.10
```

---

# Connectivity Validation

Test remote PowerShell access:

```bash id="winrm05"
evil-winrm -i 192.168.1.10 -u user -p password
```

---

# Authentication Review

WinRM supports multiple authentication methods:

* Basic authentication (if enabled)
* NTLM authentication
* Kerberos authentication (domain environments)
* Certificate-based authentication

### Assessment questions:

* Is Basic authentication enabled without encryption?
* Are weak credentials used?
* Is domain authentication enforced?
* Are privileged accounts accessible via WinRM?

---

# User Access Review

Identify users allowed to use WinRM:

Common groups:

* Administrators
* Remote Management Users
* Domain Admins
* IT support groups

Windows check:

```bash id="winrm06"
net localgroup "Remote Management Users"
```

---

# PowerShell Remote Session Check

If access is available:

```powershell id="winrm07"
Enter-PSSession -ComputerName 192.168.1.10
```

or:

```powershell id="winrm08"
Invoke-Command -ComputerName 192.168.1.10 -ScriptBlock { whoami }
```

---

# Misconfiguration Checks

Look for:

* WinRM exposed to all network segments
* Basic authentication enabled without HTTPS
* Weak or reused credentials
* Excessive users in Remote Management Users group
* No account lockout policies
* Privileged accounts accessible via WinRM
* Lack of logging or monitoring

---

# Lateral Movement Relevance

WinRM is widely used for:

* Remote PowerShell execution
* Administrative automation
* Lateral movement between Windows systems
* Post-compromise command execution
* Domain-wide system management

---

# WinRM Assessment Workflow

```text id="winrm09"
Identify WinRM Service (5985/5986)
            |
            v
Validate Connectivity
            |
            v
Determine Authentication Methods
            |
            v
Check User Access Groups
            |
            v
Test Remote PowerShell Access
            |
            v
Identify Misconfigurations
            |
            v
Assess Lateral Movement Potential
            |
            v
Review Logging and Monitoring
            |
            v
Document Findings
```

---

# Key Takeaways

* WinRM provides powerful remote management capabilities in Windows environments.
* Misconfigurations can enable full remote command execution.
* It is heavily used in Active Directory environments for administration and automation.
* Proper authentication and network restrictions are critical for security.
* Monitoring PowerShell activity is essential for detecting abuse.
