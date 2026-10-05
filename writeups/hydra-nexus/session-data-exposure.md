# Session Data Exposure — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

GET /session/data

## Vulnerability

Session Information Exposure

## Unauthenticated Test

Requesting `/session/data` without a session returned:

- session_id: not_set
- user_data: {}

The endpoint also exposed request metadata including the User-Agent,
Host, and remote address.

## Authenticated Test

After authenticating as the lab's normal `user` account, the same endpoint
returned populated server-side session information:

- user_id: 2
- username: user
- role: user

The response also exposed the session cookie through the `session_id` and
request headers.

## Impact

An endpoint that exposes server-side session contents and session metadata
can disclose information that should remain internal to the application.

Depending on the application's session design and secrets, excessive
session exposure may increase the risk of session compromise or
information disclosure.

## Root Cause

The endpoint directly serializes session contents and request information
into a JSON response without restricting access or minimizing the data
returned.

## Remediation

- Remove or restrict `/session/data` in production.
- Never expose session contents to ordinary clients.
- Do not return session cookies in API responses.
- Return only information strictly required for diagnostics.
- Protect diagnostic endpoints with strong authentication and authorization.
- Avoid exposing sensitive request metadata unnecessarily.

## Evidence

Authenticated request:

GET /session/data

The response contained:

```json
{
  "user_data": {
    "role": "user",
    "user_id": 2,
    "username": "user"
  }
}
