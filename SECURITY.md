# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this GitHub Action, please report it responsibly:

1. **Do not** open a public issue
2. Email security@releaserun.com with details
3. We'll respond within 48 hours

## How This Action Works

This action runs `releaserun` CLI to scan your dependencies. It makes outbound API calls to:
- `endoflife.date` (EOL data)
- `osv.dev` (vulnerability data)

No source code or dependency contents are transmitted. Only package names and versions are checked against public databases.
