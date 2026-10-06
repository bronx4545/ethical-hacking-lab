# Insecure Deserialization — WebSploit `deserial-gate`

## Target

http://127.0.0.1:5022

## Vulnerability

Insecure Python pickle deserialization

## Input

`user_prefs` cookie

## Normal Behavior

The application creates a `UserPrefs` object, serializes it with Python
`pickle`, Base64-encodes it, and stores it in the `user_prefs` cookie.

## Vulnerable Behavior

On subsequent requests, the application:

1. Reads the `user_prefs` cookie.
2. Base64-decodes the value.
3. Passes the decoded bytes directly to `pickle.loads()`.

Because pickle deserialization can invoke attacker-controlled object
reconstruction methods, this creates a remote-code-execution condition.

## Proof of Concept

A controlled pickle payload was created whose `__reduce__()` method invokes:

`os.system("touch /tmp/deserial_rce_confirmed")`

The payload was Base64-encoded and supplied as the `user_prefs` cookie.

## Verification

After requesting:

`http://127.0.0.1:5022/`

the following check succeeded:

```bash
docker exec deserial-gate test -f /tmp/deserial_rce_confirmed
