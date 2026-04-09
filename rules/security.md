# Security Checklist

These security rules must be verified before any code is committed or shipped.

## Before Every Commit

- [ ] No hardcoded API keys, passwords, or tokens in source code.
- [ ] All external input validated and sanitized.
- [ ] Parameterized queries for all database operations — no string concatenation.
- [ ] Sanitized HTML output — no raw user content rendered as HTML.
- [ ] Authz/Authn checked for every sensitive path — no unprotected endpoints.
- [ ] Error messages scrubbed of internal details — no stack traces, DB schema, or file paths exposed to users.

## Additional Checks for Sensitive Code

- [ ] CSRF protection for state-changing actions.
- [ ] Rate limiting on public and authentication endpoints.
- [ ] HTTPS enforced for all API calls.
- [ ] Secrets stored in environment variables or secure vaults — never in source.
- [ ] Dependencies audited for known vulnerabilities before shipping.
- [ ] No `eval()`, `exec()`, or equivalent with user-controlled input.
