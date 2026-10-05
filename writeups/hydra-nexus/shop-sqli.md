# Shop SQL Injection — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

GET /shop

## Parameter

`search`

## Vulnerability

SQL Injection — UNION-based data extraction

## Baseline

A normal search for:

`laptop`

returned the Laptop product and showed the SQL query constructed by the
application.

## Proof of Concept

The following test input was supplied to the `search` parameter:

```text
%' ) UNION SELECT username,password,email,role,'' FROM users-- 
```

The application accepted the input and returned data originating from the
`users` table.

## Result

The response contained records for:

* `admin`
* `user`

This confirms that the SQL query could be extended with a `UNION SELECT`
against another database table.

The returned values appeared in product fields because the application
renders all query results using the expected five-column product layout.

## Root Cause

The application directly concatenates the `search` parameter into the SQL
statement instead of using parameterized queries.

## Impact

An attacker can potentially retrieve data from other database tables,
including sensitive account information.

## Remediation

* Use parameterized SQL queries/prepared statements.
* Never concatenate user input into SQL statements.
* Restrict database-account privileges.
* Avoid exposing SQL queries through debug output.
* Handle database errors without returning internal query details.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
