# 11. RDP Assessment

## Overview

Remote Desktop Protocol (RDP) is a Microsoft protocol that provides graphical remote access to Windows systems.

RDP is commonly used by administrators, IT support teams, and end users to remotely manage workstations and servers. Because RDP provides full interactive access to a system, it is considered one of the most sensitive remote administration services.

The objective of this assessment is to identify RDP services, review authentication controls, validate exposure, and identify security risks associated with remote access.

---

## Objectives

* Identify RDP services
* Determine RDP service configurations
* Validate service accessibility
* Review authentication mechanisms
* Identify exposed administrative interfaces
* Review authorized users
* Assess risks associated with remote access
* Document findings and remediation recommendations

---

# RDP Service Identification

## Scan RDP Port

```bash
nmap -p 3389 192.168.1.10
```

Example Output:

```text
PORT     STATE SERVICE
3389/tcp open  ms-wbt-server
```

---

## Service Detection

```bash
nmap -sV -p 3389 192.168.1.10
```

---

# RDP Configuration Enumeration

Determine RDP security settings.

```bash
nmap --script rdp-enum-encryption -p 3389 192.168.1.10
```

Potential findings:

* Network Level Authentication (NLA)
* Supported encryption methods
* SSL/TLS support
* Security layer configuration

---

# Connectivity Validation

Verify RDP accessibility.

```bash
xfreerdp /v:192.168.1.10
```

Assessment questions:

* Is the service reachable?
* Is authentication required?
* Is Network Level Authentication enabled?
* Is access restricted by network location?

---

# User Access Review

Identify users authorized to access the system through RDP.

Review:

* Administrators
* Remote Desktop Users
* Service accounts
* Shared accounts

Potential findings:

* Excessive privileged access
* Dormant accounts
* Unnecessary administrative users

Example:

```cmd
net localgroup "Remote Desktop Users"
```

---

# Authentication Review

Common authentication mechanisms:

* Local Windows accounts
* Domain accounts
* Multi-Factor Authentication (MFA)
* Smart card authentication

Assessment questions:

* Are strong passwords enforced?
* Is MFA enabled?
* Are shared accounts used?
* Are privileged accounts allowed to log in remotely?

---

# Brute-Forcing RDP

Brute-forcing a single user with a wordlist:

```bash
hydra -l [username] -P [path/to/password_list.txt] rdp://[target_ip]
```

To attack a list of usernames against a list of passwords:

```bash
hydra -L [path/to/usernames_list.txt] -P [path/to/password_list.txt] rdp://[target_ip]
```

---

# Session Review

If authorized access is available, review active sessions.

```cmd
query user
```

or:

```cmd
qwinsta
```

Potential findings:

* Active administrator sessions
* Disconnected sessions
* Excessive concurrent sessions

---

# Common Misconfigurations

Look for:

* RDP enabled when not required
* Network Level Authentication disabled
* Weak passwords
* Shared administrative accounts
* Excessive users in Remote Desktop Users group
* RDP exposed to untrusted networks
* Lack of MFA
* No account lockout policies

---

# RDP Assessment Workflow

```text
Identify RDP Service
            |
            v
Determine Service Configuration
            |
            v
Review RDP Security Settings
            |
            v
Validate Connectivity
            |
            v
Review Authentication Controls
            |
            v
Identify Authorized Users
            |
            v
Review Active Sessions
            |
            v
Brute-Forcing RDP
            |
            v
Identify Misconfigurations
            |
            v
Document Findings
```
