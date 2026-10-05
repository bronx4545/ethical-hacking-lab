# OS Command Injection — WebSploit `shell-inject`

## Target

http://127.0.0.1:5002

## Vulnerability

OS Command Injection

## Parameter

target

## Normal Input

127.0.0.1

## Proof of Concept

127.0.0.1; id

## Result

The application executes the intended `ping` command and then executes
the injected `id` command.

## Root Cause

The application concatenates user-controlled input into an operating-system
command and executes it with `shell=True`.

## Impact

An attacker may be able to execute arbitrary operating-system commands with
the privileges of the application process.

## Remediation

- Avoid `shell=True`.
- Use `subprocess.run()` with an argument list.
- Validate and constrain the expected hostname/IP input.
- Apply least-privilege execution.

## Scope

127.0.0.1:5002 — local WebSploit training environment.
