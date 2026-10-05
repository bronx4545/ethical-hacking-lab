# Admin Panel SQL Injection — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

GET /admin

## Parameter

`user_id`

## Vulnerability

SQL Injection in the Admin Panel

## Baseline

The following request was tested:

`GET /admin?user_id=999`

The application returned:

`User not found.`

## Proof of Concept

The following value was supplied:

`999 OR 1=1`

Request:

`GET /admin?user_id=999%20OR%201=1`

The application then returned:

```text
User Details (ID: 999 OR 1=1)
Username: admin
Role: admin
```

The change from `User not found` to a real user demonstrates that the
SQL expression was altered by the injected input.

## Root Cause

The `user_id` parameter is directly interpolated into a SQL query instead of
being passed as a parameterized value.

## Impact

An attacker can manipulate the database query and retrieve records that
would not be returned for the original identifier.

The impact is increased because the `/admin` endpoint does not require
authentication in this training application.

## Remediation

* Use parameterized SQL queries.
* Treat `user_id` strictly as an integer.
* Enforce authentication before exposing administrative functionality.
* Enforce authorization for administrative resources.
* Do not expose database queries or internal debug information.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
