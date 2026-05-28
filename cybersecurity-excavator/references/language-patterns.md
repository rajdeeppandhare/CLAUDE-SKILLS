# Language-Specific Vulnerability Patterns

## JavaScript / Node.js
- `eval()`, `Function()`, `setTimeout(string)` → RCE risk
- `innerHTML`, `document.write()`, `outerHTML` with user data → XSS
- `require('child_process').exec(userInput)` → Command injection
- Prototype pollution: `obj[userKey] = userVal` without key validation
- `Math.random()` for security tokens → use `crypto.randomBytes()`
- JWT: verify signature AND expiry; never `alg: none`
- `res.redirect(req.query.url)` → Open redirect
- Express: missing `helmet()`, missing rate limiter on auth routes

## Python
- `exec()`, `eval()`, `pickle.loads()` with user data → RCE
- `os.system()`, `subprocess.call(shell=True)` with user input → Command injection
- String formatting in SQL: `f"SELECT * WHERE id={user_id}"` → SQLi
- `yaml.load()` without Loader → use `yaml.safe_load()`
- Flask: `debug=True` in production, `SECRET_KEY` hardcoded
- Django: missing `{% csrf_token %}`, `mark_safe()` with user data → XSS
- Path traversal: `open(f"/uploads/{filename}")` without sanitization

## PHP
- `$_GET`/`$_POST` directly in queries → SQLi
- `echo $_GET['input']` without escaping → XSS
- `include($_GET['page'])` → LFI/RFI
- `system()`, `exec()`, `passthru()` with user input → RCE
- `md5()`/`sha1()` for passwords → use `password_hash()`
- `$_FILES['file']['type']` for validation → easily spoofed, check mime properly
- Object injection via `unserialize()` on user input

## Java
- `Runtime.exec()` with user input → Command injection
- String concatenation in JDBC queries → SQLi (use PreparedStatement)
- `ObjectInputStream.readObject()` on untrusted data → deserialization RCE
- XXE: XML parsing without disabling external entities
- Spring: `@RequestMapping` without auth annotations, CSRF disabled
- Logging user input directly → log injection

## Go
- `os/exec` with unsanitized user input → Command injection
- `fmt.Sprintf` in SQL queries → SQLi (use `database/sql` with `?` placeholders)
- `math/rand` for tokens → use `crypto/rand`
- Goroutine data races on shared state without mutex
- `html/template` vs `text/template` — use html/template for HTML output

## SQL (general)
- Concatenated queries: `"SELECT * FROM users WHERE id=" + id`
- Stored procedures that still concatenate → still vulnerable
- Wildcard permissions: `GRANT ALL ON *.* TO user`
- No row-level security on multi-tenant data

## Infrastructure / Config
- Docker: `--privileged`, running as root, exposed Docker socket
- Kubernetes: missing NetworkPolicies, overprivileged ServiceAccounts, secrets in env vars
- nginx/Apache: directory listing on, server version exposed, missing security headers
- `.env` files committed to git or in public webroot
- AWS: public S3 buckets, overly permissive IAM, security groups `0.0.0.0/0` on all ports
