# 07. DNS Assessment

## Overview

The Domain Name System (DNS) is a critical service that translates hostnames into IP addresses and enables communication across networks.

In Active Directory environments, DNS plays an essential role in authentication, service discovery, domain operations, and system management.

---

## Objectives

* Identify DNS servers
* Enumerate DNS records
* Identify internal hosts and services
* Review DNS zone configurations
* Assess information disclosure risks
* Identify misconfigurations
* Document findings and security impact

---

# DNS Service Identification

## Scan DNS Port

```bash
nmap -p 53 192.168.1.10
```

Example Output:

```text
PORT   STATE SERVICE
53/tcp open  domain
53/udp open  domain
```

---

## Service Version Detection

```bash
nmap -sV -p 53 192.168.1.10
```

---

# DNS Enumeration

## Query a DNS Server

```bash
nslookup
server 192.168.1.10
```

---

## Resolve a Hostname

```bash
nslookup dc01.corp.local 192.168.1.10
```

Example:

```text
Name: dc01.corp.local
Address: 192.168.1.5
```

---

## Query Specific Record Types

### A Records

```bash
nslookup web01.corp.local
```

### MX Records

```bash
nslookup -type=mx corp.local
```

### NS Records

```bash
nslookup -type=ns corp.local
```

### SRV Records

```bash
nslookup -type=srv _ldap._tcp.dc._msdcs.corp.local
```

---

# Active Directory DNS Enumeration

DNS is heavily used by Active Directory.

Important records:

| Record Type | Purpose         |
| ----------- | --------------- |
| A           | Host resolution |
| PTR         | Reverse lookup  |
| MX          | Mail servers    |
| NS          | Name servers    |
| SRV         | Domain services |
| CNAME       | Aliases         |

---

## Discover Domain Controllers

```bash
nslookup -type=srv _ldap._tcp.dc._msdcs.corp.local
```

Potential Findings:

* Domain controllers
* Site information
* Internal server names

---

## Discover Kerberos Services

```bash
nslookup -type=srv _kerberos._tcp.corp.local
```

---

# Reverse DNS Lookups

Reverse lookups may reveal hostnames.

```bash
nslookup 192.168.1.10
```

Example:

```text
10.1.168.192.in-addr.arpa
Name: fileserver01.corp.local
```

---

# Zone Transfer Assessment

A DNS zone transfer allows secondary DNS servers to synchronize records.

Misconfigured servers may expose all DNS records.

Example assessment:

```bash
dig axfr corp.local @192.168.1.10
```

Potential Findings:

* Internal hosts
* Domain controllers
* File servers
* Printers
* Management systems

---

# Information Disclosure Review

Look for:

* Internal hostnames
* Server naming conventions
* Domain controller names
* Backup servers
* Management systems
* Development environments

Examples:

```text
dc01.corp.local
fileserver01.corp.local
backup01.corp.local
vpn01.corp.local
```

---

# DNS Assessment Workflow

```text
Identify DNS Server
          |
          v
Determine DNS Version
          |
          v
Query DNS Records
          |
          v
Enumerate AD DNS Records
          |
          v
Identify Domain Controllers
          |
          v
Perform Reverse Lookups
          |
          v
Review Zone Configuration
          |
          v
Assess Information Disclosure
          |
          v
Identify Misconfigurations
          |
          v
Assess Security Impact
          |
          v
Document Findings
```