# JWT Weak Secret Privilege Escalation — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoints

POST /api/auth

GET /api/protected

## Vulnerability

Weak JWT Signing Secret / JWT Privilege Escalation

## Baseline

A legitimate user token obtained through:

`POST /api/auth`

contained:

```json id="u3p5v6"
{
  "username": "user",
  "role": "user"
}
```

The legitimate token was accepted by:

`GET /api/protected`

with the response indicating:

`role: user`

## Attack

The application uses the weak HS256 signing secret:

`secret`

A new token was generated with:

```json id="if0r6n"
{
  "username": "user",
  "role": "admin"
}
```

and signed with HS256 using the recovered weak secret.

## Result

The forged token was accepted by `/api/protected`:

```json id="p7zqf4"
{
  "message": "Access granted to protected resource",
  "success": true,
  "user_data": {
    "role": "admin",
    "username": "user"
  }
}
```

This demonstrates privilege escalation through creation of a forged JWT.

## Root Cause

The application uses a short, predictable JWT signing secret and does not
protect the signing key adequately.

## Impact

An attacker who obtains the signing secret can create valid JWTs with
modified authorization claims and potentially escalate privileges.

## Remediation

* Use a cryptographically random high-entropy signing key.
* Store signing keys securely outside application source code.
* Rotate compromised signing keys immediately.
* Use explicit algorithm allowlists.
* Validate authorization claims server-side.
* Do not treat a JWT claim such as `role` as trustworthy without a valid,
  securely verified token.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
