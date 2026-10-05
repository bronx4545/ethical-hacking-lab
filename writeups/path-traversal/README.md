# Path Traversal — WebSploit `maze-walker`

## Target

http://127.0.0.1:5003

## Endpoint

GET /

## Parameter

file

## Vulnerability

Path Traversal / Directory Traversal

## Normal Request

/?file=hello.txt

Result:

Hello! This is a public file.

## Proof of Concept

/?file=../../etc/hostname

## Result

The application returned the contents of a file outside the intended
`files` directory.

## Root Cause

The application joins the user-controlled filename with the `files`
directory without verifying that the resulting path remains inside the
intended directory.

## Impact

An attacker may be able to read files outside the application's intended
document repository, depending on the privileges and filesystem layout of
the application.

## Remediation

- Resolve the requested path to a canonical absolute path.
- Verify that the resolved path remains inside the permitted base directory.
- Use an allowlist of permitted filenames where practical.
- Never trust user-controlled filesystem paths.

## Scope

127.0.0.1:5003 — local WebSploit training environment.
