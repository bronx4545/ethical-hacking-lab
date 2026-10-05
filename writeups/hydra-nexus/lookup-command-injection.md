# DNS Lookup Command Injection — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /tools/lookup

## Parameter

`domain`

## Vulnerability

OS Command Injection

## Baseline

A normal request using:

```text
domain=localhost
type=A
```

caused the application to construct the command:

```text
nslookup -type=A localhost
```

The container returned an error because `nslookup` was not installed. This
does not affect the injection test because the command construction itself
was observed.

## Proof of Concept

The following controlled input was supplied:

```text
domain=localhost; id
type=A
```

The application returned:

```text
uid=0(root) gid=0(root) groups=0(root)
```

This confirms that the injected `id` command was executed by the server.

## Impact

An attacker may execute arbitrary operating-system commands through the
`domain` parameter.

The application process runs as root inside the training container, which
increases the potential impact within that environment.

## Root Cause

The application incorporates the user-controlled `domain` value into a shell
command without adequate validation or safe argument handling.

## Remediation

* Do not construct shell commands from untrusted input.
* Use subprocess APIs with argument arrays and shell execution disabled.
* Strictly validate domain names.
* Run the application as a non-root user.
* Apply least-privilege controls to the container.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
