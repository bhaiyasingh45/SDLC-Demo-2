# Exercise 12 — Security Review

**Duration**: 4 minutes  
**Copilot Feature**: Security Prompt File  
**Goal**: Run a structured OWASP Top 10 security review of the codebase and fix critical findings.

---

## Background

Security is not an afterthought in modern SDLC — it's a gate before deployment. GitHub Copilot can audit code against the **OWASP Top 10** and propose fixes. By encoding the review checklist in a prompt file, the entire team runs the same consistent security audit every sprint.

This exercise uses the pre-built `.github/prompts/security-review.prompt.md` to drive the review.

---

## Step 1 — Open the Security Review Prompt

Open `.github/prompts/security-review.prompt.md` and read the OWASP checklist it contains. This file was created in the workshop setup.

Notice it covers:
- A01: Broken Access Control
- A02: Cryptographic Failures
- A03: Injection attacks
- A04: Insecure Design
- And remaining OWASP categories through A10: SSRF

---

## Step 2 — Run the Security Review

In Copilot Chat (default Agent), type `/` and select **Security Review**, or send:

```
Run .github/prompts/security-review.prompt.md against src/. For each OWASP finding, list file/line, severity, plain-language risk, and exact fix. Prioritize A01 and A03.
```

---

## Step 3 — Apply Critical Fixes

After the review, send this prompt to apply the most important fixes:

```
Apply CRITICAL and HIGH findings only. For each, show before/after and why it fixes the issue. Leave MEDIUM/LOW as // SECURITY-TODO comments.
```

Watch Copilot work through the findings and edit the source files in `src/`.

---

## Step 4 — Verify Common Patterns

Send this targeted prompt to check the most common injection vectors:

```
Audit src/repositories/ for injection risk: parameterized SQL or safe JSON access, validated IDs, bounded page/limit. Report each file SAFE/VULNERABLE and fix vulnerable patterns.
```

---

## Step 5 — Generate a Security Findings Report

Send:

```
Create doc/security-review-report.md with date, reviewer, files reviewed, findings table (Severity | OWASP | File | Status), fixed issues, open MEDIUM/LOW items, and overall posture.
```

---

## Key Takeaway

> Security built into the workflow — not bolted on after — is what OWASP recommends as "Shift Left Security." By encoding the OWASP checklist in a reusable prompt file, your team runs the same security review every sprint, every PR. The prompt captures institutional knowledge about *what to look for* so no security engineer knowledge needs to be in every developer's head.

---

**Next**: [Exercise 13 — Build & Debug](exercise-13-build-debug.md)
