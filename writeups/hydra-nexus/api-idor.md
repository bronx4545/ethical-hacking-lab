# API IDOR — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

GET /api/users/<id>

## Vulnerability

Broken Object-Level Authorization (IDOR/BOLA)

## Proof of Concept

Requesting:

GET /api/users/1

returned the administrator's user record, including sensitive fields such
as `password_hash` and `api_key`, without authentication or authorization.

Requesting:

GET /api/users/2

returned a different user's record using the same endpoint and only changing
the numeric identifier.

## Impact

An attacker can enumerate user IDs and access information belonging to
other accounts.

## Root Cause

The endpoint retrieves a user by ID but performs no authorization check to
determine whether the requester is allowed to access that object.

## Remediation

- Enforce object-level authorization for every user record.
- Return only fields required by the client.
- Never expose password hashes or API keys through normal user APIs.
- Use non-predictable identifiers where appropriate, but do not rely on
  obscurity instead of authorization.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
