# 06. Telnet Assessment

## Overview

Telnet is a legacy remote administration protocol that provides command-line access to remote systems. Unlike SSH, Telnet transmits authentication credentials and session data in clear text, making it unsuitable for modern environments.

Although Telnet has largely been replaced by SSH, it is still found on legacy systems, network devices, printers, embedded systems, and industrial environments.

The objective of this assessment is to identify Telnet services, review authentication controls, validate exposure, and identify security risks associated with its use.

---

## Objectives

* Identify Telnet services
* Determine Telnet software versions
* Validate service accessibility
* Review authentication mechanisms
* Identify exposed administrative interfaces
* Assess risks associated with clear-text communications
* Document findings and remediation recommendations

---

# Telnet Service Identification

## Scan Telnet Port

```bash
nmap -p 23 192.168.1.10
```

Example Output:

```text
PORT   STATE SERVICE
23/tcp open  telnet
```

---

## Service Version Detection

```bash
nmap -sV -p 23 192.168.1.10
```

Example:

```text
PORT   STATE SERVICE VERSION
23/tcp open  telnet
```

---

# Banner Enumeration

Telnet services often expose useful banner information.

```bash
nc 192.168.1.10 23
```

Example:

```text
Welcome to Router01
Login:
```

Potential findings:

* Device hostname
* Vendor information
* System type
* Administrative interface disclosure

---

# Connectivity Validation

Verify Telnet accessibility.

```bash
telnet 192.168.1.10
```

Assessment questions:

* Is the service reachable?
* Is authentication required?
* Is access restricted by network location?

---

# Authentication Review

Review how users authenticate to the service.

Assessment considerations:

* Username/password authentication
* Shared administrative accounts
* Default credentials
* Account lockout controls
* Multi-factor authentication availability

Questions:

* Are unique administrator accounts used?
* Are default credentials removed?
* Are login attempts monitored?

---

# User Access Review

Identify users authorized to access the service.

Review:

* Administrative accounts
* Service accounts
* Shared accounts
* Vendor accounts

Potential findings:

* Excessive privileged access
* Dormant accounts
* Unnecessary administrative users

### Scenario

A Telnet service is exposed on a server:

```bash
telnet 192.168.1.10
```

You see a login prompt:

```bash
Welcome to DeviceX
Login:
```

### Testing different usernames

#### Case 1: Invalid username

```bash
Login: fakeuser
Password: test123
Login incorrect
```

#### Case 2: Valid username

```bash
Login: admin
Password: test123
Password incorrect
```

---

# Common Misconfigurations

Look for:

* Telnet enabled when SSH is available
* Default credentials not removed
* Shared administrative accounts
* Excessive network exposure
* Lack of management network segmentation
* Legacy devices using Telnet by default

---

# Telnet Assessment Workflow

```text
Identify Telnet Service
            |
            v
Determine Service Version
            |
            v
Review Banner Information
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

```

---

# Hardening Recommendations

* Disable Telnet where possible
* Replace Telnet with SSH
* Restrict administrative access to management networks
* Remove default credentials
* Implement unique administrator accounts
* Enable centralized logging and monitoring
* Apply network segmentation