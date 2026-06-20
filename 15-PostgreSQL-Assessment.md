# 15. PostgreSQL Assessment

## Overview

PostgreSQL is a powerful open-source relational database management system widely used in enterprise applications, web platforms, cloud environments, and internal business systems.

Because databases often contain sensitive information, PostgreSQL servers are high-value assets during internal security assessments. The objective of this assessment is to identify PostgreSQL services, review authentication controls, enumerate accessible databases, assess security configurations, and identify potential risks.

---

## Objectives

* Identify PostgreSQL servers
* Determine PostgreSQL versions
* Validate connectivity
* Enumerate databases
* Review user accounts and privileges
* Assess authentication mechanisms
* Identify sensitive data exposure
* Document security findings

---

# PostgreSQL Service Identification

## Scan PostgreSQL Port

```bash
nmap -p 5432 192.168.1.10
```

Example Output:

```text
PORT     STATE SERVICE
5432/tcp open  postgresql
```

---

## Service Version Detection

```bash
nmap -sV -p 5432 192.168.1.10
```

Example:

```text
5432/tcp open  postgresql PostgreSQL 15.2
```

---

# Connectivity Validation

Verify database accessibility.

```bash
psql -h 192.168.1.10 -U postgres
```

Assessment questions:

* Is the service reachable?
* Is authentication required?
* Is remote access permitted?

---

# Brute-Forcing Postgresql

```bash
hydra -l [username] -P /usr/share/wordlists/rockyou.txt -t 4 [target_IP] postgres
```

---

# Database Enumeration

After obtaining authorized access, enumerate available databases.

```sql
\l
```

Example:

```text
postgres
employees
inventory
finance
```

Connect to a Database:

Example:

```sql
\c employees
```

Output:

```text
You are now connected to database "employees"
```

---

# Table Enumeration

List tables within a database.

```sql
\dt
```

Example:

```text
users
employees
products
orders
```

---

# User Enumeration

Review database accounts.

```sql
\du
```

Example:

```text
 Role name  | Attributes
------------+-----------------------------------
 postgres   | Superuser, Create role, Create DB
 app_user   |
 report_user| Cannot login
```

Assessment questions:

* Are unused accounts present?
* Are service accounts properly restricted?
* Are default accounts still active?

---

# Privilege Review

Review assigned privileges for a specific user:

```bash
\du app_user
```

Examples:

* SUPERUSER
* CREATEDB
* CREATEROLE
* LOGIN

Potential risks:

* Excessive privileges
* Unnecessary administrative accounts
* Shared database accounts

---

# Configuration Review

Important configuration files:

```text
postgresql.conf
pg_hba.conf
```

Review:

* Authentication methods
* Remote access configuration
* Network restrictions
* Logging configuration

---

# Authentication Assessment

Common authentication methods:

* Password authentication
* SCRAM-SHA-256
* LDAP
* Kerberos
* Certificate-based authentication

Assessment questions:

* Are strong authentication methods used?
* Are legacy authentication methods disabled?
* Are privileged accounts protected?

---

# Sensitive Data Review

Databases frequently contain:

* Customer information
* Employee records
* Financial data
* Application credentials
* API keys

Examples:

```text
users
customers
employees
payments
credentials
```

---

# PostgreSQL Assessment Workflow

```text
Identify PostgreSQL Service
            |
            v
Determine Version
            |
            v
Validate Connectivity
            |
            v
Brute-Forcing Postgresql
            |
            v
Enumerate Databases
            |
            v
Enumerate Tables
            |
            v
Review User Accounts
            |
            v
Review Privileges
            |
            v
Review Configuration
            |
            v
Assess Authentication
            |
            v
Identify Sensitive Data
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

---

# Key Takeaways

* PostgreSQL databases often store highly sensitive information.
* User privileges and authentication mechanisms should be reviewed carefully.
* Excessive permissions are a common security issue.
* Database exposure should be minimized through network segmentation and access controls.
* Continuous monitoring helps detect unauthorized access and suspicious activity.
