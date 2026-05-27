# Exercise 07 — Create an Implementation Prompt File

**Duration**: 3 minutes  
**Copilot Feature**: Prompt Files (`.prompt.md`)  
**Goal**: Package your implementation plan into a reusable prompt file that any team member can invoke.

---

## Why This Matters

Right now, only you know how to ask Copilot for a well-structured implementation plan. A **Prompt File** (`.prompt.md`) solves that — it captures your instructions as a saved, versioned file in `.github/prompts/` so any teammate can invoke the same prompt with a single `/` command.

Instead of one person knowing "the right way to ask," the whole team gets a consistent `/itms-implementation-plan` command they can run anytime.

---

## Step 1 — Understand the Prompt File Format

A `.prompt.md` file looks like this:

```markdown
---
name: Prompt Name
description: "When to use this prompt"
---

# Prompt content here

Instructions for Copilot...
```

The `description` field is what appears in the Copilot Chat command palette when you type `/`.

---

## Step 2 — Inspect the Pre-Built Prompt File

Open `.github/prompts/implementation-plan.prompt.md` and notice three things:
- The **YAML frontmatter** — `name` and `description` fields that register it in the command palette
- The **FRD reference rule** — every task must cite an `FR-ID` or `US-ID`
- The **Background Agent flag** — tasks are marked when they are long and self-contained

This is the generic version. Next, you will create a project-specific one.

---

## Step 3 — Create a Project-Specific Version

In Copilot Chat, send:

```
Create .github/prompts/itms-implementation-plan.prompt.md for ITMS. It must read #frd.md and #tsd.md, use [YOUR STACK], reference src/routes/, src/services/, src/repositories/, src/models/, include data/migration and OpenAPI tasks, flag Background Agent candidates, and output H2 phases plus a table: ID | Task | Effort | FRD Ref | Parallel? | Background Agent?
```

> Replace `[YOUR STACK]` with your choice from Exercise 01.

---

## Step 4 — Use and Compare

1. In Copilot Chat, type `/` — the command palette opens
2. Type `itms` — select **itms-implementation-plan** and press Enter
3. Compare the output with `.github/prompts/implementation-plan.prompt.md`:
   - The generic file works for any project
   - Your new file targets ITMS folder conventions, stack, and tooling specifically

> This is the difference between a shared team library and a project-specific standard.

---

**Next**: [Exercise 08 — Build APIs with Local Agent](exercise-08-api-local-agent.md)

> 🟡 **Optional stop available**: If your GitHub repository and Personal Access Token are configured, try [Exercise 14 — Create GitHub Issues via MCP](exercise-14-github-issues.md) before continuing to Exercise 08.
