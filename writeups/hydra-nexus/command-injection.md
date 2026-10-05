# Command Injection — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /tools/ping

## Parameter

`host`

## Vulnerability

OS Command Injection

## Proof of Concept

The following controlled input was supplied:

```text
host=127.0.0.1; id
count=1
```

The application returned:

```text
uid=0(root) gid=0(root) groups=0(root)
```

This confirms that the `host` parameter is incorporated into an operating-system command and that injected commands are executed by the application.

## Impact

An attacker may be able to execute arbitrary operating-system commands with
the privileges of the application process.

In this lab, the application process runs as root inside the Docker
container, significantly increasing the impact within the container.

## Root Cause

The application passes attacker-controlled input to an OS command without
proper command separation or input validation.

## Remediation

* Avoid shell command construction with untrusted input.
* Use subprocess APIs with argument arrays rather than shell interpretation.
* Validate host/IP input against a strict allowlist.
* Disable shell execution where it is not required.
* Run the application as a non-root user.
* Apply container-level least privilege.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
