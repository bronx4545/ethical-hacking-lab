# Server-Side Request Forgery — WebSploit `trojan-relay`

## Target

http://127.0.0.1:5012

## Vulnerable Endpoint

POST /fetch

## Vulnerability

Server-Side Request Forgery (SSRF)

## Proof of Concept

URL:

http://10.6.6.32:5012/

## Result

The application made a server-side HTTP request to its own internal
Docker-network address and returned the response with:

Status: 200 OK

## Internal Access Test

Target:

http://10.6.6.31:5006/

This target is another intentionally vulnerable application on the
WebSploit Docker network.

## Root Cause

The `/fetch` endpoint accepts a user-controlled URL and passes it to an
HTTP client without validating or restricting the destination.

## Impact

An attacker may be able to make the server access internal services that
are not directly exposed to the client.

## Remediation

- Allowlist permitted destination hosts.
- Restrict protocols to HTTP/HTTPS where appropriate.
- Block loopback, link-local, and private network destinations.
- Apply network-level egress restrictions.
- Do not return unrestricted internal responses to clients.

## Scope

127.0.0.1:5012 and the isolated WebSploit Docker network.
