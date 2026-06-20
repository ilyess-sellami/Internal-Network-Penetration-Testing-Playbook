# 14. MySQL Assessment

## Overview

MySQL is one of the most widely used relational database management systems in web applications, enterprise systems, and cloud environments.

Because it often stores sensitive application data such as user accounts, credentials, financial records, and business logic, MySQL servers are high-value targets during internal security assessments.

---

## Objectives

* Identify MySQL services
* Determine MySQL version
* Validate connectivity
* Enumerate databases
* Review users and privileges
* Assess authentication controls
* Identify sensitive data exposure
* Document findings and risks

---

# MySQL Service Identification

## Scan MySQL Port

```bash id="mysql01"
nmap -p 3306 192.168.1.10
```

Example Output:

```text id="mysql02"
PORT     STATE SERVICE
3306/tcp open  mysql
```

---

## Service Version Detection

```bash id="mysql03"
nmap -sV -p 3306 192.168.1.10
```

Example:

```text id="mysql04"
3306/tcp open  mysql MySQL 8.0.34
```

---

# Connectivity Validation

Test database access:

```bash id="mysql05"
mysql -h 192.168.1.10 -u root -p
```

or:

```bash id="mysql06"
mysql -h 192.168.1.10 -u app_user -p
```

---

# Brute-Forcing MySQL

```bash
hydra -l [username] -P /usr/share/wordlists/rockyou.txt mysql://[target_IP]
```

---

# Database Enumeration

After successful authentication:

```sql id="mysql07"
SHOW DATABASES;
```

Example:

```text id="mysql08"
information_schema
mysql
performance_schema
employees
webapp
```

---

# Select a Database

```sql id="mysql09"
USE employees;
```

---

# Table Enumeration

```sql id="mysql10"
SHOW TABLES;
```

Example:

```text id="mysql11"
users
orders
payments
employees
logs
```

---

# View Table Structure

```sql id="mysql12"
DESCRIBE users;
```

Example:

```text id="mysql13"
id INT
username VARCHAR
password VARCHAR
email VARCHAR
created_at TIMESTAMP
```

---

# User Enumeration

```sql id="mysql14"
SELECT User, Host FROM mysql.user;
```

Example:

```text id="mysql15"
root         localhost
app_user     %
backup_user   192.168.1.%
```

---

# Privilege Review

Check privileges assigned to users:

```sql id="mysql16"
SHOW GRANTS FOR 'root'@'localhost';
```

Example:

```text id="mysql17"
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost'
```

---

## Check Global Privileges

```sql id="mysql18"
SELECT * FROM information_schema.user_privileges;
```

---

# Authentication Review

MySQL supports multiple authentication methods:

* Native password authentication
* Plugin-based authentication (caching_sha2_password)
* LDAP (enterprise setups)
* PAM authentication (Linux environments)

Assessment questions:

* Are weak or default credentials used?
* Is root accessible remotely?
* Are service accounts over-privileged?
* Are password policies enforced?

---

# Sensitive Data Review

MySQL databases commonly contain:

* User credentials
* Application secrets
* Financial transactions
* Session data
* API keys

Examples:

```text id="mysql19"
users
admin_accounts
payments
logs
sessions
```

---

# Configuration Review

Important configuration files:

```text id="mysql20"
my.cnf
mysqld.cnf
```

Key settings:

* bind-address
* skip-networking
* user privileges
* logging configuration
* authentication plugin settings

---

# MySQL Assessment Workflow

```text id="mysql21"
Identify MySQL Service
            |
            v
Determine Version
            |
            v
Validate Connectivity
            |
            v
Brute-Forcing MySQL
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