---
description: Audit changed code for security vulnerabilities (Python-focused)
argument-hint: "<PR number, branch, or path> (optional; defaults to current diff)"
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
---

# Security Review

Audit the changes in **$ARGUMENTS** (default: the uncommitted/branch diff) for
security issues. Focus only on the changed code and the code paths it touches.

## What to check

- **Injection** — SQL/NoSQL built via string formatting instead of parameterized
  queries; shell calls via `os.system`/`subprocess` with `shell=True` and
  untrusted input; `eval`/`exec`/`pickle.loads` on external data.
- **Secrets** — hardcoded API keys, passwords, tokens, private keys; secrets
  logged or returned in responses; `.env` values committed.
- **AuthN/AuthZ** — missing permission checks on new endpoints; trusting
  client-supplied IDs/roles; broken object-level authorization.
- **Input validation** — unvalidated request data, path traversal in file
  operations, SSRF via user-supplied URLs, unbounded input sizes.
- **Crypto & data handling** — weak hashing (md5/sha1 for passwords), missing
  TLS verification (`verify=False`), insecure randomness (`random` for tokens
  instead of `secrets`), sensitive data in logs.
- **Dependencies** — newly added packages: are they reputable and pinned?
- **Deserialization & templates** — unsafe YAML load, autoescaping disabled in
  templates (XSS), mass-assignment.

## Output

For each finding: **severity** (Critical / High / Medium / Low), file:line, the
vulnerability, a realistic exploit scenario in one sentence, and the remediation.
If you find nothing, say so explicitly and note what you inspected. Do not write
or run exploit code.
