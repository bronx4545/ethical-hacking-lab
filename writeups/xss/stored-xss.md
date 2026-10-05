# Stored XSS — WebSploit `phantom-script`

## Target

http://127.0.0.1:5011/comments

## Vulnerability

Stored Cross-Site Scripting (XSS)

## Input

comment

## Proof of Concept

<script>alert('Stored-XSS')</script>

## Behavior

The application stores the submitted comment and subsequently renders it
into the comments page without HTML escaping.

## Impact

Stored JavaScript can execute in a user's browser when the vulnerable page
is viewed.

## Root Cause

Untrusted comment content is inserted directly into generated HTML.

## Remediation

HTML-escape untrusted output and apply context-appropriate output encoding.
Use a secure templating approach that escapes variables by default.

## Scope

127.0.0.1:5011 — local WebSploit training environment.
