# Exercise 13 — Build & Debug with the Local Agent

**Duration**: 5 minutes  
**Copilot Feature**: Local Agent + Terminal Tool  
**Goal**: Use Copilot to build the application in the terminal, interpret errors, and iteratively fix them until all tests pass.

---

## Background

The local agent can run terminal commands, read their output, diagnose errors, and make code changes — all in one conversation. This creates a tight **build → error → fix → build** loop that compresses debugging time significantly.

---

## Step 1 — Start a Build-and-Fix Session

In Copilot Chat (Agent mode), send:

```
Build and run ITMS: install dependencies, build/compile, start the app, and fix errors at root cause. For each error show message, cause, fix, and re-run. Continue until GET /api/v1/health returns status ok.
```

The agent will run commands in the terminal, read the output, and iterate on fixes. You'll see it:
1. Run `npm install` (or equivalent)
2. Run `npm run build`(or equivalent)
3. Fix any TypeScript errors or import issues
4. Start the server
5. Test the health endpoint

---

## Step 2 — Run Unit Tests

Once the app builds, send:

```
Run unit tests using npm run test:unit or the stack equivalent. For failures, show test + message, decide code vs test bug, fix root cause, rerun until green, then show coverage.
```

---

## Step 3 — Debug a Specific Scenario

Try a deliberate debugging exercise. Send:

```
Use curl to POST /api/v1/tasks with a valid assignedUserId from src/data/users.json, due date next Friday, priority HIGH, title "Implement Payment API". Then GET the returned ID. Show commands, responses, logs, and fix if POST is not 201 TO_DO or GET is 404.
```

If the request fails, Copilot will:
1. Look at the server logs
2. Trace the request through the controller → service → repository
3. Identify the bug (e.g., date validation blocking valid future dates, or missing FK constraint handling)
4. Apply the fix and re-test

---

## Step 4 — Run Integration Tests

Send:

```
Start the app if needed, then run npm run test:integration or stack equivalent. For failures, compare with doc/frd.md Gherkin criteria, fix root cause, and rerun.
```

---

## Step 5 — Verify End-to-End Flow

Send a final validation prompt:

```
Run a curl E2E flow: list seeded tasks, create HIGH task for valid user due next Friday, get it and confirm TO_DO, patch to IN_PROGRESS, confirm history, patch to COMPLETED, filter COMPLETED and confirm it appears. Show commands/responses and fix any error.
```

---

## Debugging Tips for Attendees

If Copilot gets stuck in a loop on a specific error:

```
Stop trying to fix [specific error]. Let's approach this differently.
What is the root cause of this error based on the stack trace? 
What are the 3 most likely causes? Eliminate them one by one.
```

If a test keeps failing after fixes:

```
Read the original acceptance criteria in doc/frd.md for [FR-ID or US-ID].
Is the test testing the right thing? Is the implementation correct per the FRD?
Tell me which is wrong — the test or the implementation — before making any changes.
```

---

## Key Takeaway

> The local agent's superpower is the **terminal + code edit + reasoning** combination. A human debugger reads an error, opens the file, makes a change, saves, rebuilds, checks the output. The local agent does this loop autonomously, maintaining context across multiple iterations. The key practice is to give it a **goal** ("all tests pass") rather than micro-managing each step.

---

**🏁 Mandatory Track Complete!**

You have completed all 13 mandatory exercises and experienced the full E2E SDLC lifecycle with GitHub Copilot — from requirements through architecture, implementation, UI, testing, and security.

> 🟡 **Optional capstone**: Continue with [Exercise 17 — IaC & CI/CD](exercise-17-iac-cicd.md) to generate Docker, Terraform/Bicep, and GitHub Actions pipelines for the ITMS application.
