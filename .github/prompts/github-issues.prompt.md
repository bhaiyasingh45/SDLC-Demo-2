---
name: GitHub Issues Generator
description: "Convert an implementation plan into GitHub Issues."
---

# GitHub Issues Generator

Convert `doc/implementation-plan.md` or the current plan into GitHub Issues ready for GitHub MCP or manual creation.

## For Each Issue, Provide

```markdown
### Issue: [Issue Title]

**Labels**: enhancement | bug | documentation | testing | infrastructure | security
**Milestone**: Phase 1 | Phase 2 | Phase N
**Assignee**: (leave blank)
**Estimated Effort**: S (< 4h) | M (4–8h) | L (8–16h)

**Description**:
[2–3 sentence description of what needs to be done and why]

**Acceptance Criteria**:
- [ ] [Verifiable criterion 1]
- [ ] [Verifiable criterion 2]
- [ ] [Verifiable criterion 3]

**Technical Notes**:
[Any technical context, file paths, or dependencies the developer needs]

**References**:
- FRD: [FR-ID]
- Depends on: #[issue-number] (if known)
```

---

After generating the list, provide GitHub MCP create-issue instructions for repository `[OWNER/REPO]` with title, full body, labels, and milestone.

---

## Grouping Rules

- Group issues by Phase (Phase 0 -> Phase 1 -> Phase 2...)
- Create one **Epic issue** per Phase to link sub-issues
- Database tasks → label `database`
- API tasks → label `backend`
- Test tasks → label `testing`
- Security tasks → label `security`
- IaC/CI/CD tasks → label `infrastructure`
