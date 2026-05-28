# OWASP Top 10 Quick Reference (2021)

## A01 - Broken Access Control
- Users acting outside intended permissions
- Signs: missing auth checks, IDOR, path traversal, CORS misconfiguration
- Check: every route/endpoint has authorization, not just authentication

## A02 - Cryptographic Failures
- Weak/missing encryption for sensitive data
- Signs: MD5/SHA1 for passwords, HTTP not HTTPS, unencrypted PII in DB
- Check: bcrypt/argon2 for passwords, TLS everywhere, encrypted sensitive fields

## A03 - Injection
- Untrusted data sent to interpreter
- Signs: string concatenation in queries, eval(), os.system(), innerHTML with user data
- Check: parameterized queries, input sanitization, output encoding

## A04 - Insecure Design
- Missing threat modeling, insecure design patterns
- Signs: no rate limiting, no fraud controls, overly permissive defaults
- Check: defense-in-depth, principle of least privilege

## A05 - Security Misconfiguration
- Default credentials, verbose errors, unnecessary features enabled
- Signs: stack traces to users, default admin passwords, directory listing on
- Check: security headers, minimal permissions, error handling

## A06 - Vulnerable & Outdated Components
- Using components with known vulnerabilities
- Signs: old package versions, unmaintained dependencies
- Check: package-lock.json versions against CVE databases

## A07 - Identification & Authentication Failures
- Weak auth implementation
- Signs: no MFA, weak passwords allowed, broken session management, predictable tokens
- Check: secure session IDs, account lockout, credential stuffing protection

## A08 - Software & Data Integrity Failures
- Code/infrastructure not verified for integrity
- Signs: unsigned updates, insecure deserialization, untrusted CDN scripts
- Check: SRI hashes for CDN, signed packages, safe deserialization

## A09 - Logging & Monitoring Failures
- Insufficient logging of security events
- Signs: no audit logs, passwords logged, no alerting on failures
- Check: log auth events, flag anomalies, don't log sensitive data

## A10 - Server-Side Request Forgery (SSRF)
- Server fetching attacker-controlled URLs
- Signs: user-supplied URLs fetched server-side, webhooks without validation
- Check: allowlists for external URLs, block internal IP ranges
