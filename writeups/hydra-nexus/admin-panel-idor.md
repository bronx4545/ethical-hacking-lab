# Admin Panel Authorization Bypass / IDOR — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

GET /admin

## Vulnerabilities

* Missing authentication / authorization
* Broken Object-Level Authorization (IDOR)
* Sensitive information disclosure

## Proof of Missing Authentication

The `/admin` endpoint was accessed without supplying a session or
authorization token.

The response displayed the administrator's record, including sensitive
account fields.

## Proof of IDOR

The request:

`GET /admin?user_id=2`

returned:

```text
User Details (ID: 2)
Username: user
Role: user
```

Changing only the `user_id` parameter changed the object returned by the
application.

## Impact

An unauthenticated attacker can access the administrative interface and
retrieve information belonging to different users by modifying the
`user_id` parameter.

The application also exposes sensitive information such as password hashes
and API keys through this interface.

## Root Cause

The endpoint does not enforce authentication or object-level authorization
before retrieving and displaying user information.

## Remediation

* Require authentication before accessing administrative functions.
* Enforce server-side authorization for every requested object.
* Do not trust client-controlled user IDs.
* Never expose password hashes or API keys in web responses.
* Separate administrative functions from ordinary user functionality.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
