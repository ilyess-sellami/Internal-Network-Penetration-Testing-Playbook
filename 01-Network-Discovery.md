# 01. Network Discovery

## Overview

Network discovery is the first phase of an internal penetration test. The objective is to identify active hosts, understand network ranges, discover accessible systems, and build an inventory of potential targets for further assessment.

This phase helps security teams understand network visibility and identify assets that may be exposed to unauthorized access.

---

## Objectives

* Identify live hosts
* Discover network segments
* Map network topology
* Identify operating systems
* Locate important infrastructure
* Build a target inventory

---

# Host Discovery

Host discovery determines which systems are currently online.

## ICMP Ping Sweep

Ping sweeps can identify hosts that respond to ICMP Echo Requests.

### Example

```bash
ping 192.168.1.10
```

### Scan a Subnet

```bash
nmap -sn 192.168.1.0/24
```

### Example Output

```text
Nmap scan report for 192.168.1.1
Host is up

Nmap scan report for 192.168.1.10
Host is up

Nmap scan report for 192.168.1.25
Host is up
```

---

# ARP Discovery

ARP discovery is highly effective on local networks because it does not rely on ICMP responses.

### Example

```bash
arp-scan --localnet
```

### Example Output

```text
192.168.1.1     00:11:22:33:44:55
192.168.1.10    aa:bb:cc:dd:ee:ff
192.168.1.20    11:22:33:44:55:66
```

---

# TCP Discovery

Some systems block ICMP but still respond to TCP probes.

### Discover Hosts Using TCP SYN

```bash
nmap -sn -PS80,443,445 192.168.1.0/24
```

### Discover Hosts Using TCP ACK

```bash
nmap -sn -PA80,443 192.168.1.0/24
```

---

# Identifying Network Ranges

Understanding network boundaries is important before conducting service enumeration.

### View Local Interface Information

Linux:

```bash
ip addr
```

Windows:

```cmd
ipconfig
```

### View Routing Information

Linux:

```bash
ip route
```

Windows:

```cmd
route print
```

---

# Identifying Important Infrastructure

During discovery, special attention should be given to systems that provide network services.

Examples:

* Domain Controllers
* DNS Servers
* DHCP Servers
* File Servers
* Application Servers
* Database Servers
* Management Systems
* Virtualization Hosts

---

# Reverse DNS Lookups

Hostnames often reveal the purpose of a system.

### Example

```bash
nslookup 192.168.1.10
```

Example output:

```text
Name: dc01.corp.local
Address: 192.168.1.10
```

---

# Operating System Fingerprinting

Operating system detection helps determine the type of host being assessed.

### Example

```bash
nmap -O 192.168.1.10
```

Example output:

```text
OS details:
Microsoft Windows Server
```

---

# Network Discovery Workflow

## Step 1 - Identify Live Hosts

Discover active hosts on the target network.

```bash
nmap -sn 192.168.1.0/24
```

Save results:

```bash
nmap -sn 192.168.1.0/24 -oN hosts.txt
```

---

## Step 2 - Discover Open Ports

Scan discovered hosts for common open ports.

```bash
nmap 192.168.1.10
```

Or scan the entire subnet:

```bash
nmap 192.168.1.0/24
```

---

## Step 3 - Identify Running Services

Determine what services are listening on open ports.

```bash
nmap -sV 192.168.1.0/24
```

Example:

```text
192.168.1.10
445/tcp open microsoft-ds
3389/tcp open ms-wbt-server

192.168.1.20
22/tcp open ssh
```

---

## Step 4 - Identify Operating Systems

Attempt operating system fingerprinting.

```bash
sudo nmap -O 192.168.1.0/24
```

Example:

```text
192.168.1.10 Windows Server
192.168.1.20 Linux
192.168.1.30 Windows 10
```

---

## Step 5 - Collect Hostnames

Perform reverse DNS lookups.

```bash
nmap -R 192.168.1.0/24
```

Example:

```text
192.168.1.10 dc01.corp.local
192.168.1.20 filesrv01.corp.local
192.168.1.30 web01.corp.local
```

---

## Step 6 - Comprehensive Discovery Scan

Combine host discovery, service detection, OS fingerprinting, and default enumeration scripts.

```bash
sudo nmap -A 192.168.1.0/24
```

This attempts to identify:

* Host status
* Open ports
* Service versions
* Operating systems
* Hostnames
* Additional service information

---

## Step 7 - Create Inventory

Document discovered systems.

| IP Address   | Hostname  | OS             | Key Services        |
| ------------ | --------- | -------------- | ------------------- |
| 192.168.1.10 | dc01      | Windows Server | SMB, LDAP, Kerberos |
| 192.168.1.20 | filesrv01 | Windows Server | SMB                 |
| 192.168.1.30 | web01     | Linux          | SSH, HTTP           |

---

## Typical Discovery Flow

```text
Target Network
      |
      v
Host Discovery (-sn)
      |
      v
Port Discovery
      |
      v
Service Detection (-sV)
      |
      v
OS Detection (-O)
      |
      v
Hostname Resolution
      |
      v
Asset Inventory
```
