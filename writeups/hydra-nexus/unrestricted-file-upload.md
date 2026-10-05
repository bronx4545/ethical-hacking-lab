# Unrestricted File Upload — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /upload

## Vulnerability

Unrestricted File Upload / Unsafe Content Hosting

## Proof of Concept

A harmless HTML file named:

`test.html`

was uploaded using the `/upload` endpoint.

The application accepted the file and reported:

`Filename: test.html`

`File path: uploads/test.html`

The application also provided a direct URL:

`/uploads/test.html`

## Verification

Requesting:

`GET /uploads/test.html`

returned the uploaded HTML content:

```html id="d6p9ei"
<html><body>Hydra upload test</body></html>
```

This demonstrates that the application accepts and stores HTML content and
then serves it directly from the upload location.

## Impact

Allowing arbitrary user-controlled content to be uploaded and served can
create security risks. Depending on how uploaded content is handled and
served, attackers may be able to host active content, overwrite files, or
combine the upload weakness with other vulnerabilities.

## Root Cause

The application accepts the original client-supplied filename and does not
perform an extension or content-type allowlist before saving the file.

## Remediation

* Restrict uploads to required file types only.
* Validate file content rather than trusting the extension.
* Generate server-side filenames.
* Store user-uploaded content outside the application's executable/web
  directory where practical.
* Serve uploads with appropriate content types and security headers.
* Disable execution of uploaded content.
* Apply strict size and permission limits.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
