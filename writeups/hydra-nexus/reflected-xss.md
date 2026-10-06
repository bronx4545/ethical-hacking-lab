# Reflected XSS — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

GET /admin

## Parameter

`debug`

## Vulnerability

Reflected Cross-Site Scripting (XSS)

## Proof of Reflection

The following payload was supplied through the `debug` parameter:

```html
<script>alert(1)</script>
```

The application reflected the payload directly into the HTML response:

```html
<p>Debug mode enabled with parameter: <script>alert(1)</script></p>
```

## Proof of Execution

The following URL was opened in the browser:

`/admin?debug=<encoded XSS payload>`

The browser executed the JavaScript and displayed an alert containing:

`1`

This confirms actual reflected XSS rather than simple HTML reflection.

## Root Cause

The application inserts the user-controlled `debug` parameter directly into
an HTML response without context-appropriate output encoding.

## Impact

An attacker may execute arbitrary JavaScript in a victim's browser within the
security context of the application origin.

Potential impacts include unauthorized actions performed through the victim's
session, page manipulation, and access to information available to scripts
running under the application's origin.

## Remediation

* HTML-encode untrusted output.
* Use context-aware output encoding.
* Avoid constructing HTML with untrusted strings.
* Implement a restrictive Content Security Policy.
* Remove debug functionality from production interfaces.
* Validate and minimize diagnostic parameters.

## Evidence

Server-side reflection was confirmed with:

```text
<script>alert(1)</script>
```

Browser-side execution was confirmed by the `alert(1)` popup.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
