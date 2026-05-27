> ✅ **Already completed by Exercise 02** — If you ran the **SDLC Docs Orchestrator** in Exercise 02, `doc/frd.md` is already generated. Skip to [Exercise 06 — Plan Mode](exercise-06-plan-mode.md), or continue here to explore the FRD Agent in isolation.

# Exercise 05 — Generate the Functional Requirements Document

**Duration**: 4 minutes  
**Copilot Feature**: FRD Custom Agent  
**Goal**: Use the FRD agent to create testable user stories and use cases from the BRD and TSD.

---

## Background

The FRD is the document development teams and QA engineers work from directly. It breaks down business requirements into **use cases**, **user stories with Gherkin acceptance criteria**, and a **permissions matrix**. The FRD Author agent synthesizes the BRD and TSD into this developer-ready document.

---

## Step 1 — Switch to the FRD Agent

1. In Copilot Chat, click the agent selector
2. Select **FRD Author**

---

## Step 2 — Send the FRD Generation Prompt

Copy and paste this prompt:

```
Read #requirement.md, #brd.md, and #tsd.md; create doc/frd.md. Include roles/permissions, UC-001..UC-006, Gherkin stories, FR catalogue with traceability, validation rules, notification triggers, and user-facing error scenarios.
```

## Checkpoint — Docs Complete ✅

You should now have:

```
doc/
├── brd.md    ← Business requirements (from Exercise 02)
├── tsd.md    ← Technical architecture (from Exercise 03)
└── frd.md    ← Functional requirements + user stories (this exercise)
```

These three documents form the **specification foundation** for all remaining exercises. Every piece of code, database schema, test, and deployment script will trace back to one of these files.

---

## Key Takeaway

> Notice how each agent built on the previous output. BRD → TSD → FRD is the natural SDLC chain, and each agent was pre-wired to read the prior document. This is the power of **document-driven agentic workflows** — Copilot maintains context across the entire specification phase without you having to copy-paste content.

---

**Next**: [Exercise 06 — Plan Mode for Implementation](exercise-06-plan-mode.md)
