# 03. FTP Assessment

## Overview

FTP (File Transfer Protocol) is a legacy file-sharing service frequently found in internal networks. It is commonly misconfigured, exposing sensitive files or allowing weak authentication controls.

The objective of this assessment is to identify insecure FTP configurations, validate access controls, and document potential data exposure risks.

---

## Objectives

* Identify FTP services on internal hosts
* Detect anonymous or weak authentication configurations
* Enumerate accessible directories and files
* Identify upload and download permissions
* Detect sensitive data exposure
* Document misconfigurations and risk impact

---

# FTP Service Identification

## Scan FTP Port

```bash id="ftp01"
nmap -p 21 192.168.1.10
```

## Service Version Detection

```bash id="ftp02"
nmap -sV -p 21 192.168.1.10
```

Example Output:

```text id="ftp03"
21/tcp open  ftp  vsftpd 3.0.3
```

---

# Connection Testing

## Basic FTP Connection

```bash id="ftp04"
ftp 192.168.1.10
```

## Banner Grabbing

```bash id="ftp05"
nc 192.168.1.10 21
```

Example:

```text id="ftp06"
220 (vsFTPd 3.0.3)
```

---

# Anonymous Access Assessment

Many FTP servers are misconfigured to allow anonymous login.

## Login Test

```bash id="ftp07"
ftp 192.168.1.10
```

Then:

```text id="ftp08"
Name: anonymous
Password: anonymous
```

### Expected Findings (if misconfigured):

* Read access to directories
* Public file exposure
* Backup or configuration files

---

# Directory Enumeration

## List Files

```bash id="ftp09"
ls -la
```

## Navigate Directories

```bash id="ftp10"
cd /backup
ls
```

---

# File Access Validation

## Download Files (Read Access Test)

```bash id="ftp11"
get backup.zip
```

## Upload Test (Write Access Check)

```bash id="ftp12"
put test.txt
```

---

# Brute-Forcing FTP

To attack a specific user against an FTP server:

```bash
hydra -l [username] -P [path/to/password_list.txt] ftp://[target_ip]
```

To attack a list of usernames against a list of passwords:

```bash
hydra -L [path/to/usernames_list.txt] -P [path/to/password_list.txt] -t 4 ftp://[target_ip]
```

use Nmap's built-in FTP brute script:

```bash
nmap --script ftp-brute -p 21 [target_ip]
```

Common location of users lists and passwords lists (Kali linux):

```bash
/usr/share/seclists/Usernames/
/usr/share/seclists/Passwords/
/usr/share/wordlists/
```

Most famous passwords list:

```bash
/usr/share/wordlists/rockyou.txt
```

# Misconfiguration Checks

Look for:

* Anonymous login enabled
* weak username or password (for example `admin`:`admin`)
* Writable public directories
* Backup files exposed (.bak, .old, .zip)
* Configuration files (.conf, .env)
* Credential storage files

---

# FTP Assessment Workflow

```text id="ftp13"
Scan FTP port (21)
        |
        v
Identify service version
        |
        v
Test connectivity
        |
        v
Check anonymous login
        |
        v
Enumerate directories
        |
        v
Test file download/upload
        |
        v
Identify sensitive data exposure
        |
        v
Brute-Forcing FTP
        |
        v
Document misconfigurations
```

---

# Hardening Recommendations

* Disable anonymous FTP access
* Enforce strong authentication
* Restrict directory permissions
* Disable write access where not needed
* Use SFTP instead of FTP
* Monitor file transfer logs