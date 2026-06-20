# 04. SMB Assessment

## Overview

Server Message Block (SMB) is a core Windows protocol used for file sharing, network communication, and administrative operations in Active Directory environments.

Due to its deep integration with Windows systems, SMB is one of the most frequently abused protocols during internal penetration testing, especially for **lateral movement, enumeration, and privilege discovery**.

The objective of this assessment is to identify SMB exposure, enumerate shares, review access controls, and detect misconfigurations that may lead to unauthorized access.

---

## Objectives

* Identify SMB services and versions
* Enumerate shared resources
* Detect anonymous or guest access
* Identify sensitive file exposure
* Map domain and host information
* Review SMB signing and security settings
* Assess lateral movement opportunities

---

# SMB Service Identification

## Scan SMB Ports

```bash
nmap -p 445 192.168.1.10
```

```bash
nmap -p 139 192.168.1.10
```

Example Output:

```text
PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

---

## Service Version Detection

```bash
nmap -sV -p 445 192.168.1.10
```

---

# SMB Enumeration

## List Available Shares

```bash
smbclient -L //192.168.1.10 -N
```

Example Output:

```text
Sharename       Type
---------       ----
ADMIN$          Disk
C$              Disk
Users           Disk
Public          Disk
```

---

## Access a Share

```bash
smbclient //192.168.1.10/Users -N
```

---

## Explore Shared Content

```bash
ls
cd Documents
get file.txt
```

---

# Anonymous / Guest Access Check

Test whether SMB allows unauthenticated access:

* Guest share access
* Read-only public shares
* Writable directories

Potential findings:

* Public file exposure
* Misconfigured shared folders
* Sensitive internal documents

---

# User and Domain Enumeration

SMB can reveal domain-level information.

```bash
nmap --script smb-enum-users -p 445 192.168.1.10
```

```bash
nmap --script smb-enum-shares -p 445 192.168.1.10
```

---

# File System Exposure

Check for:

* Configuration files
* Backup archives
* Credential files
* Deployment scripts
* Database exports

Common risky files:

```text
*.bak
*.zip
*.sql
*.config
*.env
```

---

# SMB Signing and Security Settings

Security posture checks:

* Is SMB signing enabled?
* Is SMBv1 disabled?
* Is guest access allowed?
* Are shares restricted by ACLs?

---

# SMB Assessment Workflow

```text
Identify SMB Service (445/139)
            |
            v
Detect Version and Configuration
            |
            v
Enumerate Shares
            |
            v
Test Anonymous / Guest Access
            |
            v
Access and Browse Shares
            |
            v
Identify Sensitive Files
            |
            v
Review Permissions and ACLs
            |

            v
Check SMB Security Settings
            |
            v
Identify Lateral Movement Paths
            |
            v
Document Findings
```