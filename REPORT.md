# WebSploit Labs — Penetration Testing Report

## 1. Executive Summary

A security assessment was performed against the intentionally vulnerable
WebSploit Labs training environment running in Docker.

The assessment identified multiple vulnerabilities across authentication,
authorization, injection, file handling, deserialization, client-side
security, API security, and business logic.

The strongest confirmed impacts included operating-system command execution
through command injection and unsafe deserialization, SQL injection,
privilege escalation through a weak JWT signing secret, broken object-level
authorization, unrestricted file upload, path traversal, and reflected XSS.

All testing described in this report was performed against the local
training environment.

## 2. Scope

Primary multi-vulnerability target:

`http://127.0.0.1:5010`

Other local WebSploit training applications were also assessed during the
exercise.

Testing was performed against intentionally vulnerable applications running
inside the local Docker environment.

## 3. Methodology

Testing followed a practical web-application security methodology:

1. Application discovery and endpoint enumeration.
2. Baseline behavior testing.
3. Parameter and input validation testing.
4. Authentication and authorization testing.
5. Injection testing.
6. File and parser security testing.
7. Deserialization testing.
8. JWT security testing.
9. Client-side security testing.
10. Evidence collection and reproducibility verification.

Only runtime-confirmed vulnerabilities are classified as confirmed findings.

## 4. Confirmed Findings

| #  | Finding                              | Severity |
| -- | ------------------------------------ | -------- |
| 1  | Python Pickle Deserialization RCE    | Critical |
| 2  | YAML Deserialization RCE             | Critical |
| 3  | OS Command Injection — Ping          | Critical |
| 4  | OS Command Injection — DNS Lookup    | Critical |
| 5  | Login SQL Injection                  | High     |
| 6  | Shop UNION SQL Injection             | High     |
| 7  | Admin SQL Injection                  | High     |
| 8  | JWT Weak-Secret Privilege Escalation | High     |
| 9  | Unauthenticated Admin Panel          | High     |
| 10 | API IDOR / BOLA                      | High     |
| 11 | Reflected XSS                        | High     |
| 12 | Upload Filename Path Traversal       | High     |
| 13 | Unrestricted File Upload             | Medium   |
| 14 | Session Data Exposure                | Medium   |
| 15 | Price Manipulation                   | Medium   |

Severity classifications are based on the demonstrated impact in the
training environment and should not be interpreted as production-specific
CVSS scores.

## 5. Critical Findings

### 5.1 Python Pickle Deserialization RCE

Endpoint:

`POST /api/data`

A controlled pickle payload was processed by the application and executed an
OS command inside the Docker container.

Verification:

`PICKLE RCE CONFIRMED`

The proof used creation of a marker file inside the container.

### 5.2 YAML Deserialization RCE

Endpoint:

`POST /api/config`

A controlled unsafe YAML payload invoked `os.system()` during deserialization.

Verification:

`YAML RCE CONFIRMED`

### 5.3 Command Injection

Endpoints:

`POST /tools/ping`

`POST /tools/lookup`

Controlled injection using `id` returned:

`uid=0(root) gid=0(root) groups=0(root)`

This demonstrated operating-system command execution by the application
process inside the container.

## 6. High-Impact Findings

### SQL Injection

SQL injection was confirmed through:

* `/login`
* `/shop`
* `/admin`

The tests demonstrated authentication bypass and retrieval of data from
other database tables.

### JWT Weak Secret

A legitimate low-privilege user token was obtained and then replaced with a
forged HS256 token using the application's weak signing secret and a modified
`role=admin` claim.

The forged token was accepted by `/api/protected`.

### Authorization Failures

The `/admin` endpoint was accessible without authentication, while
`/api/users/<id>` allowed access to other users' records.

### Reflected XSS

The `/admin?debug=` parameter reflected an unescaped `<script>` element and
executed JavaScript in the browser.

## 7. Medium Findings

The assessment also confirmed:

* unrestricted HTML file upload;
* upload filename path traversal;
* session information exposure;
* client-controlled purchase price manipulation.

## 8. Findings Not Reproduced

The following source-labelled vulnerabilities were tested but were not
confirmed in the observed runtime:

### XXE

The application returned:

`undefined entity &xxe;`

when an external entity was supplied.

### Order IDOR

The `/order/1`, `/order/2`, and `/order/3` pages were reachable, but no order
records were returned during testing.

### JWT `none` Algorithm

An unsigned token caused PyJWT 2.15.1 to raise:

`InvalidKeyError: When alg = "none", key value must be None.`

The result was a server error rather than successful authentication bypass.

These findings should remain classified as not reproduced rather than
reported as confirmed vulnerabilities.

## 9. Remediation Priorities

### Immediate

Remove unsafe pickle and YAML deserialization.

Eliminate shell execution involving untrusted input.

Replace dynamically constructed SQL statements with parameterized queries.

Rotate and replace weak JWT signing secrets.

Require authentication and object-level authorization for administrative and
user-specific resources.

### High Priority

Implement secure file-upload handling and prevent path traversal.

Encode untrusted data before inserting it into HTML.

Protect sensitive API and session information.

Perform all transaction-price calculations server-side.

### General

Apply least privilege to application and container processes.

Remove production debug interfaces.

Minimize sensitive information returned by APIs.

Implement centralized input validation and authorization controls.

## 10. Evidence Management

Temporary session cookies, private keys, uploaded test files, local wordlists,
and the external `websploit/` source directory were excluded from the Git
repository through `.gitignore`.

The repository contains 29 Markdown write-ups documenting individual
exercises and findings.

## 11. Conclusion

The assessment demonstrated a broad range of intentionally vulnerable web
application behaviors in the WebSploit training environment.

Several findings resulted in demonstrated administrative access or
server-side command execution inside the training containers.

The assessment also distinguished between vulnerabilities that were
successfully reproduced and source-labelled behaviors that could not be
reproduced in the actual runtime.
