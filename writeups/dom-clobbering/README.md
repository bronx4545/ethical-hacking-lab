# DOM Clobbering — WebSploit `proto-pollute`

## Target

http://127.0.0.1:5004

## Vulnerability

DOM Clobbering

## Lab

Lab 1: DOM Clobbering

## Vulnerable Logic

The application inserts user-controlled HTML using `innerHTML` and then
checks:

```javascript
if (window.config && window.config.scriptUrl) {
    scriptUrl = window.config.scriptUrl;
}
