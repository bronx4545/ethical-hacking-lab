cat >> reconnaissance/juice-shop/recon.md <<'EOF'

## FTP Directory Discovery

The /ftp endpoint exposes a directory listing.

Observed files:

- quarantine/
- acquisitions.md
- announcement_encrypted.md
- coupons_2013.md.bak
- eastere.gg
- encrypt.pyc
- incident-support.kdbx
- legal.md
- package-lock.json.bak
- package.json.bak
- suspicious_errors.yml

Observation:
The application exposes multiple files and backup artifacts through a browsable directory.
Further investigation should determine which files are intentionally vulnerable training targets and whether sensitive information is exposed.
EOF
