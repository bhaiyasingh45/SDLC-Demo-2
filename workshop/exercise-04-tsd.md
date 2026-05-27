> ✅ **Already completed by Exercise 02** — If you ran the **SDLC Docs Orchestrator** in Exercise 02, `doc/tsd.md` is already generated. Skip to [Exercise 06 — Plan Mode](exercise-06-plan-mode.md), or continue here to explore the TSD Agent in isolation.

> **Language inherited from Exercise 01**: The TSD agent reads `.github/copilot-instructions.md` and will design the architecture (tech stack, data layer, API patterns) using your chosen language and framework automatically.

# Exercise 04 — Generate the Technical Specification Document

**Duration**: 4 minutes  
**Copilot Feature**: TSD Custom Agent  
**Goal**: Use the TSD agent to design the system architecture from the BRD.

---

## Background

The TSD bridges business requirements and engineering. Your **TSD Author** agent acts as a Solutions Architect — it reads the BRD, proposes technology choices, designs the data model and API contracts, and produces a document the development team can build from.

---

## Step 1 — Switch to the TSD Agent

1. In Copilot Chat, click the agent selector
2. Select **TSD Author**

---

## Step 2 — Send the TSD Generation Prompt

Copy and paste this prompt:

```
Read #brd.md and #requirement.md; create doc/tsd.md. Include Mermaid architecture + ER diagrams, REST endpoints for auth/users/tasks/assignment/dependencies/reporting, stack rationale, OWASP Top 10 security, CI/CD, and BRD ID traceability.
```

---

## Step 3 — Monitor the Plan

The TSD agent will plan its approach. Look for:
- It plans to read BOTH `#brd.md` AND `#requirement.md`
- It mentions Mermaid diagrams
- It is NOT writing application code

Approve the plan.

---

## Step 4 — Ask for a Specific Architecture Decision (Optional)

Try this follow-up to see how the agent reasons:

```
Update doc/tsd.md Integration Points for async SendGrid email, Microsoft Teams manager alerts, and a sequence diagram for task-status notification flow.
```

---

## Key Takeaway

> The architecture designed here becomes the **source of truth** for the next exercises. Every API you build, every table you create, and every test you write will trace back to `doc/tsd.md`. This is how Copilot becomes a true SDLC co-pilot — not just a code generator, but an architectural collaborator.

---

**Next**: [Exercise 05 — Generate FRD](exercise-05-frd.md)
