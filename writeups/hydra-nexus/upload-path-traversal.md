# Upload Filename Path Traversal — WebSploit `hydra-nexus`

## Target

http://127.0.0.1:5010

## Endpoint

POST /upload

## Vulnerability

Unrestricted File Upload with Filename Path Traversal

## Baseline

A harmless file named `upload-test.txt` was successfully uploaded to the
application's upload service.

The application reported the expected path:

`uploads/upload-test.txt`

## Proof of Concept

The same harmless file was uploaded using the attacker-controlled filename:

`../traversal-test.txt`

The application reported:

`uploads/../traversal-test.txt`

The file was then verified inside the Docker container at:

`/app/traversal-test.txt`

This demonstrates that the `../` path segment was honored and allowed the
file to escape the intended `uploads` directory.

## Impact

An attacker may be able to write files outside the intended upload
directory. Depending on filesystem permissions and the surrounding
application, this can lead to file overwrite, unauthorized file placement,
or further compromise.

## Root Cause

The server uses the client-supplied filename directly:

```python
filename = file.filename
filepath = os.path.join('uploads', filename)
```

No normalization, canonical-path validation, or safe filename handling is
performed before saving the file.

## Remediation

* Use `werkzeug.utils.secure_filename()` or equivalent safe filename handling.
* Reject absolute paths and path separators.
* Resolve the final canonical path and verify it remains inside the upload
  directory.
* Store uploaded files under generated server-side filenames where possible.
* Restrict upload directory permissions.
* Separate uploaded content from executable application directories.

## Evidence

The successful traversal test created:

`/app/traversal-test.txt`

with the expected harmless test content.

## Scope

127.0.0.1:5010 — local WebSploit training environment.
