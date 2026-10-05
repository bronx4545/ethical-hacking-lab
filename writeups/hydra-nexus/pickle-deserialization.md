# Insecure Deserialization — Python Pickle RCE

## Target

http://127.0.0.1:5010

## Endpoint

POST /api/data

## Vulnerability

Insecure Python Pickle Deserialization

## Proof of Concept

The endpoint accepts a Base64-encoded serialized Python object.

A controlled pickle object was constructed so that its `__reduce__()` method
invoked:

```python
os.system("touch /tmp/hydra_pickle_rce")
```

The resulting Base64 payload was submitted through the `data` parameter.

## Server Response

The application returned:

```json
{
  "success": true,
  "deserialized_object": "0",
  "object_type": "<class 'int'>"
}
```

The value `0` is the return value of the executed `os.system()` call.

## Verification

The following command was used to verify the controlled marker:

```bash
docker exec hydra-nexus test -f /tmp/hydra_pickle_rce
```

Result:

```text
PICKLE RCE CONFIRMED
```

This demonstrates arbitrary command execution during unsafe pickle
deserialization inside the training container.

## Root Cause

The application Base64-decodes attacker-controlled input and passes the
result directly to `pickle.loads()`.

## Impact

An attacker who can submit a malicious pickle payload may execute arbitrary
Python/operating-system commands with the privileges of the application
process.

## Remediation

* Never use `pickle.loads()` on untrusted input.
* Prefer data-only formats such as JSON.
* Validate and authenticate serialized client data.
* Run the application with least privilege.
* Apply appropriate container and filesystem restrictions.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
