# JWT `none` Algorithm Bypass — WebSploit `token-tower`

## Target

http://127.0.0.1:5020

## Vulnerability

JWT `none` algorithm acceptance / Signature Bypass

## Endpoint

GET /api/admin/flag

## Initial Token

A legitimate `guest` JWT was obtained from:

POST /api/auth

The original payload contained:

- user: guest
- user_id: 2
- role: guest

## Attack

A forged JWT was created with:

```json
{
  "alg": "none",
  "typ": "JWT"
}
