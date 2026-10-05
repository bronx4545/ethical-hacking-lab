# JWT JWK Header Injection — WebSploit `token-tower`

## Target

http://127.0.0.1:5020

## Vulnerability

JWT JWK Header Injection

## Endpoint

POST /api/verify

## Attack

A new RSA key pair was generated locally.

The attacker's public key was embedded into the JWT header as:

```json
{
  "alg": "RS256",
  "typ": "JWT",
  "jwk": {
    "kty": "RSA",
    "n": "<attacker-controlled modulus>",
    "e": "AQAB"
  }
}
