# 11. MSSQL Assessment

## Overview

Microsoft SQL Server (MSSQL) is a relational database management system widely used in enterprise environments, business applications, ERP platforms, and Active Directory infrastructures.

Because MSSQL often stores sensitive business data and frequently integrates with Windows authentication, it is a high-value target during internal security assessments.

---

## Objectives

* Identify MSSQL services
* Determine MSSQL version
* Validate connectivity
* Enumerate databases
* Review users and privileges
* Assess authentication controls
* Identify sensitive data exposure
* Document findings and risks

---

# MSSQL Service Identification

## Scan MSSQL Port

```bash
nmap -p 1433 192.168.1.10
```

Example Output:

```text
PORT     STATE SERVICE
1433/tcp open  ms-sql-s
```

---

## Service Version Detection

```bash
nmap -sV -p 1433 192.168.1.10
```

Example:

```text
1433/tcp open  ms-sql-s Microsoft SQL Server 2019
```

---

# Connectivity Validation

Connect using sqlcmd:

```bash
sqlcmd -S [target_IP] -U [username] -P [password]
```

or:

```bash
sqsh -S [target_IP] -U [username] -P [password]
```

---

# Brute-Forcing MSSQL

```bash
hydra -l [username] -P /usr/share/wordlists/rockyou.txt mssql://[target_IP]
```

Using a username list:

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt mssql://[target_IP]
```

---

# Database Enumeration

After successful authentication:

```sql
SELECT name FROM master..sysdatabases;
```

Example:

```text
master
model
msdb
tempdb
HR
Finance
Inventory
```

---

# Select a Database

```sql
USE HR;
```

---

# Table Enumeration

```sql
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_TYPE='BASE TABLE';
```

Example:

```text
Employees
Users
Payroll
Departments
```

---

# View Table Structure

```sql
EXEC sp_columns Employees;
```

Example:

```text
EmployeeID
FirstName
LastName
Salary
Department
```

---

# User Enumeration

List SQL users:

```sql
SELECT name
FROM sys.sql_logins;
```

Example:

```text
sa
app_user
report_user
backup_user
```

---

# Privilege Review

Review server roles:

```sql
SELECT
    sp.name,
    sp.type_desc,
    sl.sysadmin
FROM sys.server_principals sp
LEFT JOIN sys.syslogins sl
ON sp.sid = sl.sid;
```

---

## Check Server Roles

```sql
EXEC sp_helpsrvrolemember 'sysadmin';
```

Example:

```text
sa
sql_admin
```

---

# Authentication Review

MSSQL supports:

* Windows Authentication
* SQL Authentication
* Active Directory Authentication
* Integrated Security

Assessment questions:

* Is the SA account enabled?
* Are weak credentials used?
* Are service accounts over-privileged?
* Is Windows Authentication enforced?

---

# Sensitive Data Review

Common sensitive tables:

* Users
* Employees
* Payroll
* Customers
* CreditCards
* APIKeys

Examples:

```text
Users
AdminAccounts
Employees
Payroll
Customers
```

---

# Configuration Review

Review server configuration:

```sql
EXEC sp_configure;
```

Important settings:

* xp_cmdshell
* Remote Access
* SQL Authentication Mode
* Audit Configuration
* Service Accounts

---

# Check Dangerous Features

## xp_cmdshell

```sql
EXEC sp_configure 'xp_cmdshell';
```

Potential finding:

```text
xp_cmdshell enabled
```

---

## Linked Servers

```sql
EXEC sp_linkedservers;
```

Potential finding:

```text
SQL01
HRSQL01
FINANCE01
```

---

# Common Misconfigurations

Look for:

* SA account enabled
* Weak SQL passwords
* Excessive sysadmin users
* xp_cmdshell enabled
* Linked servers improperly configured
* Unrestricted remote access
* Outdated MSSQL versions

---

# MSSQL Assessment Workflow

```text
Identify MSSQL Service
            |
            v
Determine Version
            |
            v
Validate Connectivity
            |
            v
Brute-Forcing MSSQL
            |
            v
Authenticate to Database
            |
            v
Enumerate Databases
            |
            v
Select Target Database
            |
            v
Enumerate Tables
            |
            v
Review Users and Privileges
            |
            v
Review Configuration
            |
            v
Check xp_cmdshell
            |
            v
Review Linked Servers
            |
            v
Identify Sensitive Data
            |
            v
Detect Misconfigurations
            |
            v
Assess Security Impact
            |
            v
Document Findings
```
