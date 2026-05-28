---
name: cybersecurity-excavator
description: >
  Deeply analyzes code, app structures, and architecture for security vulnerabilities. Use this skill whenever a user wants to: scan code for vulnerabilities, audit an app or system for security issues, check for common attack vectors (XSS, SQLi, IDOR, broken auth, exposed secrets, etc.), review API security, assess file/folder structure for risks, or get a plain-English security report with severity ratings and fix suggestions. Trigger this skill for any request involving "security audit", "find vulnerabilities", "is this code safe", "security review", "check for exploits", "penetration test my code", "secure my app", or any variation of wanting to find weaknesses in code or system design — even if the user doesn't use technical terms.
---

# Cybersecurity Excavator

A defensive security skill that digs through code, app structures, and architecture to surface hidden vulnerabilities — with clear explanations and actionable fixes.

> ⚠️ **Scope**: This skill is purely **defensive and educational**. It identifies and explains vulnerabilities so they can be fixed. It never generates exploit code, attack payloads, or offensive tooling.

---

## What This Skill Does

Given code snippets, file structures, configs, API definitions, or app descriptions, the Excavator:

1. **Excavates** – Systematically scans for vulnerability patterns
2. **Rates** – Assigns severity: 🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Low / ℹ️ Info
3. **Explains** – Describes *why* each issue is dangerous in plain English
4. **Fixes** – Provides concrete remediation with corrected code examples
5. **Summarizes** – Gives an overall security posture score and priority action list

---

## Vulnerability Categories to Always Check

### 🔴 Critical (check first)
- **Secrets in code**: hardcoded API keys, passwords, tokens, private keys
- **SQL Injection**: unsanitized user input in queries
- **Remote Code Execution**: eval(), exec(), shell injection, deserialization flaws
- **Authentication bypass**: missing auth checks, predictable tokens, broken JWT handling

### 🟠 High
- **XSS (Cross-Site Scripting)**: unescaped user input rendered in HTML
- **IDOR (Insecure Direct Object Reference)**: missing ownership checks on resources
- **Broken Access Control**: users accessing resources they shouldn't
- **Insecure file uploads**: no type/size validation, uploads to public dirs
- **SSRF**: user-controlled URLs fetched server-side without validation

### 🟡 Medium
- **Missing rate limiting**: login, API, password reset endpoints
- **Weak/missing CSRF protection**: state-changing requests without tokens
- **Sensitive data exposure**: PII/tokens in logs, URLs, error messages
- **Insecure dependencies**: outdated packages with known CVEs (flag if version numbers visible)
- **Missing security headers**: CSP, HSTS, X-Frame-Options, etc.

### 🟢 Low / ℹ️ Info
- **Verbose error messages**: stack traces, internal paths exposed to users
- **Missing input validation**: length, type, format checks
- **Weak password policy**: no complexity requirements
- **Missing HTTPS enforcement**
- **Overly permissive CORS**

---

## Output Format

Always structure the response as:

```
## 🔍 Cybersecurity Excavation Report

**Target**: [what was analyzed]
**Languages/Stack**: [detected stack]
**Overall Security Posture**: [score /10 + one-line summary]

---

### 🚨 Findings ([N] total)

#### [SEVERITY EMOJI] [SEVERITY] — [Vulnerability Name]
**Location**: [file/line/function if known]
**What it is**: [plain English explanation, 1-2 sentences]
**Why it's dangerous**: [real-world impact, what an attacker could do]
**Fix**:
[corrected code or concrete steps]

---

### ✅ What's Done Well
[acknowledge security positives if any]

### 🎯 Priority Action List
1. [Most critical fix first]
2. ...

### 📚 Further Reading
[1-2 relevant OWASP links max]
```

---

## Analysis Approach

### For code snippets:
- Read line by line, flag any of the vulnerability patterns above
- Pay special attention to: user input entry points, database queries, auth logic, file operations, external calls

### For file/folder structures:
- Flag: `.env` files in public dirs, exposed config files, sensitive filenames, overly permissive directory layouts
- Check: presence of security middleware, auth layers, input validation modules

### For app descriptions (no code):
- Ask clarifying questions about: auth mechanism, data storage, user input handling, external integrations
- Provide architecture-level risk assessment

### For multiple files:
- Read the references directory for language-specific vulnerability patterns if needed
- Prioritize entry points: routes, controllers, auth middleware, database models

---

## Tone Guidelines

- **Plain English first**: explain what the vulnerability means before using technical names
- **No fear-mongering**: be matter-of-fact, not dramatic
- **Constructive**: every finding comes with a fix
- **Honest about limits**: if you can't fully assess something (e.g., need runtime context), say so
- **Never generate**: working exploits, attack payloads, or malicious code — redirect to the fix instead

---

## Edge Cases

- **"Is this safe?"** with no code → ask them to share the code or describe the architecture
- **Obfuscated/minified code** → note limitation, do best-effort analysis, flag for manual review
- **Infrastructure/DevOps** (Dockerfiles, k8s configs, nginx configs) → apply same framework, check for exposed ports, overprivileged containers, hardcoded creds
- **Frontend-only code** → focus on XSS, sensitive data in localStorage, exposed API keys, insecure postMessage
- **Already-known vulnerability** (user says "I know about X, check for others") → acknowledge X, continue scanning for others

---

## Reference Files

- `references/owasp-top10.md` — Quick reference for OWASP Top 10 patterns (load if doing a comprehensive audit)
- `references/language-patterns.md` — Language-specific vulnerability patterns for Python, JS, PHP, Java, Go (load when analyzing a specific language in depth)
