# Exercise 06 — Plan Mode: Generate Implementation Plan

**Duration**: 4 minutes  
**Copilot Feature**: Plan Mode (Copilot Edits)  
**Goal**: Use Plan Mode to let Copilot reason through the full implementation strategy before writing any code.

---

## Why This Matters

Without Plan Mode, Copilot jumps straight into writing code the moment you send a prompt. For a large feature that touches 10+ files, that is a gamble — you can't course-correct until after the fact.

**Plan Mode flips the order**: Copilot reads your documents, maps out every file and task it intends to touch, and shows you the plan first. You review it, adjust it if needed, and only then approve execution. Think of it as a "dry run" before any file is changed.

---

## Step 1 — Switch to Plan Mode

In Copilot Chat:
1. Click the **mode selector** at the bottom-left of the chat panel
2. Select **Plan** (this may appear as `Plan` or the pencil/plan icon)

> You should see a visual indicator that Plan mode is active.

---

## Step 2 — Send the Implementation Planning Prompt

This prompt instructs Copilot to read your FRD and TSD and produce a phased plan — **without creating any files yet**.

Copy and paste this prompt:

```
Read #frd.md and #tsd.md. Create doc/implementation-plan.md with phases 0-5: setup, task management, reporting/filters, notifications, exports, testing/security/docs. For each task include ID, title, S/M/L effort, FRD/US reference, parallel/sequential, and Background Agent candidate flag. Do not create code.
```

---

## Step 3 — Review and Save the Plan

Copilot will display the full plan in chat before touching any file. Review it:

- [ ] All 5 phases are covered
- [ ] Phase 0 includes project scaffolding tasks
- [ ] Tasks reference FRD IDs
- [ ] Some tasks are flagged as Background Agent candidates
- [ ] Effort estimates seem reasonable (Phase 1 ≈ 3–5 days total)

Once satisfied, click **Open in Editor** in the Copilot response — this saves the plan as `doc/implementation-plan.md` in your workspace.

---

## Verify

Open `doc/implementation-plan.md` and confirm:

- [ ] All phases are present
- [ ] Tasks have IDs, effort estimates, and FRD references
- [ ] Background Agent candidates are flagged
- [ ] Total effort per phase is summarized

---

> This plan is the input for the next exercise — you will package it into a reusable prompt file your whole team can invoke.

**Next**: [Exercise 07 — Create Implementation Prompt File](exercise-07-implementation-prompt.md)
