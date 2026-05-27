---
name: Security Review
description: "Run an OWASP Top 10 security review and prioritize fixes."
---

# Security Review — OWASP Top 10 Analysis

Review the codebase against OWASP Top 10 (2021).

## Scope

Scan all source files in `src/` and configuration files in the workspace root and `infra/`.

## Review Checklist

Check: A01 access control/RBAC/CORS/path traversal; A02 password hashing, secrets, TLS/JWT strength; A03 SQL/command/template/XSS injection; A04 rate limits and server-side business rules; A05 debug output, methods, security headers; A06 dependency CVEs; A07 sessions/login logging/MFA; A08 dependency integrity and CI/CD secrets; A09 auth/security logging; A10 SSRF allowlists and internal-network blocking.

---

## Output Format

For each finding:

```
### [SEVERITY: CRITICAL/HIGH/MEDIUM/LOW] — A0X: [Category]

**File**: `src/path/to/file.ts` (line N)
**Issue**: [Description of the vulnerability]
**Risk**: [What an attacker can do]
**Fix**:
[Specific code change or configuration fix]
```

---

## After the Review

Summarize findings as Severity | Category | File | Status, prioritize Critical -> High -> Medium -> Low, apply CRITICAL/HIGH fixes, and add SECURITY-TODO comments for MEDIUM/LOW items.
