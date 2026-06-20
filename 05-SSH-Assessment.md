# 05. SSH Assessment

## Overview

Secure Shell (SSH) is one of the most common remote administration protocols in Linux, Unix, network appliances, and cloud environments.

While SSH is generally secure when properly configured, weak credentials, poor key management, outdated software, and excessive permissions can expose systems to unauthorized access.

The objective of this assessment is to identify SSH services, review authentication controls, validate access restrictions, and document potential security weaknesses.

---

## Objectives

* Identify SSH services
* Determine SSH versions
* Review authentication methods
* Identify weak or insecure configurations
* Validate account access controls
* Assess SSH key management practices
* Document findings and risks

---

# SSH Service Identification

## Scan SSH Port

```bash
nmap -p 22 192.168.1.10
```

Example Output:

```text
22/tcp open ssh
```

---

## Service Version Detection

```bash
nmap -sV -p 22 192.168.1.10
```

Example:

```text
22/tcp open ssh OpenSSH 9.3
```

---

# Banner Enumeration

SSH banners often reveal useful information.

```bash
nc 192.168.1.10 22
```

Example:

```text
SSH-2.0-OpenSSH_9.3
```

---

# Connection Validation

Verify that SSH access is available.

```bash
ssh user@192.168.1.10
```

Review:

* Login prompt behavior
* MFA requirements
* Access restrictions
* Warning banners

---

# Authentication Review

Determine which authentication methods are supported.

Examples:

* Password authentication
* Public key authentication
* Multi-factor authentication
* Keyboard-interactive authentication

Assessment questions:

* Is password authentication enabled?
* Is MFA enforced?
* Are service accounts restricted?
* Are shared accounts used?

---

# Brute-Forcing SSH

Brute-forcing a single user with a wordlist:

```bash
hydra -l [username] -P [path/to/password_list.txt] ssh://[target_ip]
```

To attack a list of usernames against a list of passwords:

```bash
hydra -L [path/to/usernames_list.txt] -P [path/to/password_list.txt] -t 4 ssh://[target_ip]
```

# SSH Key Assessment

Review SSH key management practices.

Common locations:

Linux:

```text
~/.ssh/
```

Common files:

```text
id_rsa
id_ed25519
authorized_keys
known_hosts
```

Assessment checks:

* Unused authorized keys
* Shared keys
* Old employee keys
* Weak key management practices

---

# User Access Review

Identify users authorized to access the system.

Examples:

```text
admin
devops
backup
service accounts
```

Review:

* Excessive administrative access
* Dormant accounts
* Shared accounts
* Vendor accounts

---

# Common Misconfigurations

Look for:

* Password authentication enabled without MFA
* Root login permitted
* Excessive authorized keys
* Shared administrator accounts
* Outdated SSH versions
* Weak access restrictions

---

# SSH Assessment Workflow

```text
Identify SSH Service
        |
        v
Determine Version
        |
        v
Review Banner Information
        |
        v
Validate Connectivity
        |
        v
Brute-Forcing SSH
        |
        v
Identify Misconfigurations
        |
        v
Document Findings
```