# 02. Port and Service Enumeration

## Overview

After identifying active hosts, the next phase is to enumerate open ports and identify the services running on those ports.

The goal is to determine:

* Which ports are accessible
* Which services are running
* Service versions
* Potential attack surfaces
* Systems that require deeper assessment

---

## Objectives

* Discover open TCP ports
* Discover open UDP ports
* Identify running services
* Identify service versions
* Categorize hosts by exposed services
* Build a service inventory

---

# TCP Port Scanning

## Scan Common Ports

```bash
nmap 192.168.1.10
```

Example Output:

```text
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
```

---

## Scan All TCP Ports

Many services run on non-standard ports.

```bash
nmap -p- 192.168.1.10
```

Example:

```text
PORT      STATE SERVICE
22/tcp    open  ssh
8080/tcp  open  http-proxy
8443/tcp  open  https-alt
```

---

# Service Version Detection

Determine the software behind open ports.

```bash
nmap -sV 192.168.1.10
```

Example:

```text
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9
80/tcp   open  http    Apache httpd 2.4.57
```

---

# Aggressive Service Detection

```bash
nmap -A 192.168.1.10
```

Attempts to identify:

* Services
* Versions
* Operating systems
* Hostnames
* Default NSE script information

---

# UDP Enumeration

Some important services use UDP.

Common UDP services:

| Port | Service |
| ---- | ------- |
| 53   | DNS     |
| 67   | DHCP    |
| 69   | TFTP    |
| 123  | NTP     |
| 161  | SNMP    |
| 500  | ISAKMP  |

---

## Scan Common UDP Ports

```bash
sudo nmap -sU 192.168.1.10
```

---

# Banner Grabbing

Banners often reveal useful information.

## HTTP

```bash
curl -I http://192.168.1.10
```

Example:

```text
Server: Apache/2.4.57
```

---

## Netcat

```bash
nc 192.168.1.10 80
```

---

# Service Categorization

Once services are identified, group systems by technology.

## Windows Services

| Port | Service |
| ---- | ------- |
| 135  | RPC     |
| 139  | NetBIOS |
| 445  | SMB     |
| 3389 | RDP     |
| 5985 | WinRM   |

---

## Linux Services

| Port | Service |
| ---- | ------- |
| 22   | SSH     |
| 111  | RPCBind |
| 2049 | NFS     |

---

## Directory Services

| Port | Service  |
| ---- | -------- |
| 389  | LDAP     |
| 636  | LDAPS    |
| 88   | Kerberos |

---

## Database Services

| Port  | Service    |
| ----- | ---------- |
| 1433  | MSSQL      |
| 3306  | MySQL      |
| 5432  | PostgreSQL |
| 27017 | MongoDB    |

---

# Full Enumeration Workflow

## Phase 1 - Host Discovery

```bash
nmap -sn 192.168.1.0/24
```

---

## Phase 2 - Quick Port Scan (All Hosts)

```bash
nmap 192.168.1.0/24
```

---

## Phase 3 - Deep Port Scan (Targeted Hosts)

```bash
nmap -p- 192.168.1.10,192.168.1.20
```

---

## Phase 4 - Service Detection

```bash
nmap -sV 192.168.1.10,192.168.1.20
```

---

## Phase 5 - OS Detection

```bash
sudo nmap -O 192.168.1.10,192.168.1.20
```

---

## Phase 6 - Full Assessment Scan

```bash
sudo nmap -A -p- -sV -O 192.168.1.10
```

---

## Phase 7 - Inventory Building

```text
IP → Hostname → OS → Open Ports → Services → Notes
```

---

# Service Inventory Example

| IP Address   | Hostname  | Services            |
| ------------ | --------- | ------------------- |
| 192.168.1.10 | dc01      | LDAP, SMB, Kerberos |
| 192.168.1.20 | filesrv01 | SMB                 |
| 192.168.1.30 | web01     | HTTP, HTTPS         |
| 192.168.1.40 | db01      | MSSQL               |

---

# Enumeration Workflow

```text
Discover Live Hosts
        |
        v
Scan TCP Ports
        |
        v
Scan UDP Ports
        |
        v
Identify Services
        |
        v
Detect Versions
        |
        v
Categorize Hosts
        |
        v
Create Service Inventory
```