# JWT Weak Secret Attack — WebSploit `token-tower`

## Target

http://127.0.0.1:5020

## Vulnerability

Weak JWT signing secret

## Algorithm

HS256

## Initial Token

A legitimate guest JWT was obtained through:

POST /api/auth

The token contained:

- user: guest
- user_id: 2
- role: guest

## Attack

A small dictionary was tested against the HS256 token.

The application accepted the candidate:

`secret123`

This demonstrated that the JWT signing secret was weak and recoverable
through a dictionary attack.

## Forged Token

A new JWT was created with:

- user: lab-attacker
- user_id: 1337
- role: admin

The token was correctly signed using the recovered secret and HS256.

## Result

The forged token was accepted by:

GET /api/admin/flag

The application returned:

`attack_method: weak_secret`

and:

`Congratulations! You've gained admin access!`

## Root Cause

The application uses a short, predictable signing secret:

`secret123`

## Impact

An attacker who recovers the signing secret can create valid JWTs with
arbitrary claims, including elevated privileges.

## Remediation

- Use a cryptographically random high-entropy signing secret.
- Store signing keys securely outside application source code.
- Rotate compromised signing keys.
- Explicitly allow only approved JWT algorithms.
- Validate authorization claims after signature verification.

## Scope

127.0.0.1:5020 — local WebSploit training environment.
