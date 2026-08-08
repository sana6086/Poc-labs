# SQL Injection — Database Exfiltration

**Severity:** Critical
**Category:** SQL Injection (SQLi)

## Overview

A SQL Injection vulnerability was identified in the application's `id` parameter due to insufficient input validation and improper handling of user-supplied input. The vulnerability allowed interaction with the backend database and demonstrated unauthorized access to sensitive application data.

## Technical Impact

Exploitation of the vulnerability enabled database enumeration and identification of the underlying **MySQL** DBMS. The application's database contained **22 tables**, including sensitive `payments`, `bookings`, and `registrations` tables.

Further enumeration of the `registrations` table identified sensitive fields including names, phone numbers, email addresses, Aadhaar numbers, and password hashes. Controlled extraction successfully demonstrated exposure of user records.

## Attack Path

```text
User-Controlled Input
        ↓
SQL Injection
        ↓
MySQL Identification
        ↓
Database Enumeration
        ↓
Table & Column Enumeration
        ↓
Sensitive Data Exposure
```

## Key Findings

* SQL Injection in the `id` parameter
* MySQL database identified
* 22 database tables enumerated
* Sensitive user information exposed
* Password hashes accessible
* Successful database-level data extraction

## Remediation

* Implement parameterized queries / prepared statements
* Perform strict server-side input validation
* Never concatenate user input directly into SQL queries
* Apply least-privilege permissions to database accounts
* Avoid exposing database errors to end users
