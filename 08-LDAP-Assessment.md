# 08. LDAP Assessment

## Overview

Lightweight Directory Access Protocol (LDAP) is a protocol used to access and manage directory services. It is commonly used in enterprise environments to store and query information such as users, groups, computers, and organizational structure.

In many environments, LDAP is a core component of identity management and is often integrated with Active Directory or other directory services.

The objective of this assessment is to identify LDAP services, enumerate directory information, review authentication mechanisms, and detect misconfigurations that may expose sensitive directory data.

---

## Objectives

* Identify LDAP services
* Determine LDAP/LDAPS configuration
* Validate service accessibility
* Enumerate directory structure
* Identify users, groups, and computers
* Review authentication mechanisms
* Detect exposed sensitive information
* Document findings and risks

---

# LDAP Service Identification

## Scan LDAP Ports

```bash
nmap -p 389,636 192.168.1.10
```

Example Output:

```text
PORT    STATE SERVICE
389/tcp open  ldap
636/tcp open  ldaps
```

---

## Service Version Detection

```bash
nmap -sV -p 389,636 192.168.1.10
```

Example:

```text
389/tcp open  ldap  OpenLDAP
636/tcp open  ssl/ldap
```

---

# LDAP Enumeration

## Basic Anonymous Bind Check

```bash
ldapsearch -x -H ldap://192.168.1.10 -s base
```

Assessment goal:

* Check if anonymous bind is allowed
* Identify exposed directory information

---

## Domain/Base DN Discovery

```bash
ldapsearch -x -H ldap://192.168.1.10 -s base namingcontexts
```

Example:

```text
dc=company,dc=local
```

---

## User Enumeration

```bash
ldapsearch -x -H ldap://192.168.1.10 -b "dc=company,dc=local" "(objectClass=person)"
```

Potential findings:

* Usernames
* Email addresses
* Account attributes
* Organizational roles

---

## Group Enumeration

```bash
ldapsearch -x -H ldap://192.168.1.10 -b "dc=company,dc=local" "(objectClass=group)"
```

---

## Computer Enumeration

```bash
ldapsearch -x -H ldap://192.168.1.10 -b "dc=company,dc=local" "(objectClass=computer)"
```

---

# Authentication Review

LDAP supports multiple authentication modes:

* Anonymous bind
* Simple bind (username/password)
* SASL authentication
* Kerberos-based authentication (in AD environments)

Assessment questions:

* Is anonymous bind enabled?
* Are credentials transmitted securely (LDAPS)?
* Are weak credentials used?
* Is LDAP exposed without encryption?

---

# LDAPS (Secure LDAP)

LDAPS runs over TLS:

```text
636/tcp
```

Benefits:

* Encrypted communication
* Protection of credentials in transit

Assessment checks:

* Is LDAPS enabled?
* Is LDAP still accessible without encryption?
* Are weak certificates used?

---

# Sensitive Information Exposure

LDAP often contains:

* Usernames
* Email addresses
* Group memberships
* Department structure
* Service accounts
* Machine accounts

Example risk:

```text
cn=admin,ou=users,dc=company,dc=local
mail=admin@company.local
```

---

# Common Misconfigurations

Look for:

* Anonymous LDAP binding enabled
* Unencrypted LDAP (389) exposed
* Excessive directory permissions
* Sensitive attributes publicly readable
* Weak or reused service account credentials
* Overly broad group membership exposure

---

# LDAP Assessment Workflow

```text
Identify LDAP Service (389/636)
            |
            v
Check Anonymous Bind
            |
            v
Determine Base DN
            |
            v
Enumerate Users
            |
            v
Enumerate Groups
            |
            v
Enumerate Computers
            |
            v
Review Authentication Mechanisms
            |
            v
Check Encryption (LDAPS)
            |
            v
Identify Sensitive Directory Data
            |
            v
Document Findings
```