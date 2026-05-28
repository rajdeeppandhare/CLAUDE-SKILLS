# 🔍 Cybersecurity Excavator — Claude Skill

> A defensive security skill for Claude that digs through your code, apps, and architecture to surface hidden vulnerabilities — with plain English explanations and actionable fixes.

---

## ✨ What It Does

Paste in any code, config, file structure, or app description and the Excavator will:

- 🕵️ **Scan** for vulnerabilities across your entire codebase or architecture
- 🎯 **Rate** every finding by severity — Critical / High / Medium / Low
- 📖 **Explain** what each vulnerability is in plain English (not just jargon)
- 🔧 **Fix** it — gives you corrected code, not just warnings
- 📋 **Summarize** with an overall security score + priority action list

---

## 🚨 Vulnerabilities It Catches

| Category | Examples |
|---|---|
| 🔴 Critical | Hardcoded secrets, SQL injection, RCE, auth bypass |
| 🟠 High | XSS, IDOR, broken access control, insecure file uploads, SSRF |
| 🟡 Medium | Missing rate limiting, CSRF, sensitive data in logs, outdated deps |
| 🟢 Low | Verbose errors, missing input validation, weak password policy |

**Supports**: JavaScript/Node.js, Python, PHP, Java, Go, SQL, Docker, Kubernetes, nginx, AWS configs and more.

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `cybersecurity-excavator.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `cybersecurity-excavator/` folder
3. Rename to `cybersecurity-excavator.skill`
4. Install via Claude.ai Settings → Skills

---

## 🚀 How to Use

Once installed, just talk to Claude naturally:

```
"Excavate this code for vulnerabilities"
"Do a security audit of my app"
"Is this code safe?"
"Check this for SQL injection"
"Review my auth logic for security issues"
```

Then paste your code — Claude will run the full excavation automatically.

---

## 📊 Example Output

```
## 🔍 Cybersecurity Excavation Report

Target: user-auth.js
Stack: Node.js / Express
Overall Security Posture: 4/10 — Several critical issues need immediate attention

---

### 🚨 Findings (4 total)

#### 🔴 Critical — Hardcoded JWT Secret
Location: Line 3
What it is: Your JWT signing secret is written directly in the code
Why it's dangerous: Anyone who reads your source code (GitHub, ex-employee, breach)
can forge any user's auth token and log in as anyone
Fix:
  // ❌ Before
  const secret = "mysupersecretkey123"

  // ✅ After
  const secret = process.env.JWT_SECRET
  // Add JWT_SECRET=<random 64-char string> to your .env file

...
```

---

## 🛡️ Important: Defensive Use Only

This skill is built purely for **finding and fixing** vulnerabilities. It will never:
- Generate working exploits or attack payloads
- Provide offensive tooling
- Help bypass security measures

This keeps it safe and focused on what actually matters: making your code more secure.

---

## 📁 Skill Structure

```
cybersecurity-excavator/
├── SKILL.md                          # Core skill logic & instructions
├── README.md                         # This file
├── cybersecurity-excavator.skill     # Installable skill package
└── references/
    ├── owasp-top10.md                # OWASP Top 10 vulnerability reference
    └── language-patterns.md          # Language-specific vuln patterns
```

---

## 🧠 Built With

- [Claude Skills](https://claude.ai) — Anthropic's skill system for extending Claude
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — Industry standard vulnerability framework

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built as a portfolio project demonstrating Claude skill development and cybersecurity knowledge.*
