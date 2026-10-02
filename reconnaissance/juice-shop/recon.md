## FTP File Access Controls

### legal.md

GET /ftp/legal.md

Result:
HTTP 200 OK

The file is directly accessible.

### package.json.bak

GET /ftp/package.json.bak

Result:
File retrieval rejected.

Server response:
"Only .md and .pdf files are allowed!"

Observation:
The application exposes non-allowed files in the directory listing but applies an extension restriction when attempting to retrieve them.

Status:
Access-control behavior identified; bypassability has not yet been tested.
