# Exercise 11 — Write Tests

**Duration**: 5 minutes  
**Copilot Feature**: Local Agent + Prompt Files + Agent Orchestration  
**Goal**: Generate unit and integration tests for the task creation and task retrieval API endpoints.

---

## Fast Path — Run Everything with the Test Orchestrator (1 Prompt)

Three specialist agents are pre-built in `.github/agents/`:

| Agent | Role | Output |
|-------|------|--------|
| `unit-test` | Unit Test Author | `tests/unit/task.service.test.*` |
| `integration-test` | Integration Test Author | `tests/integration/tasks.api.test.*` |
| `test-orchestrator` | Orchestrator | Runs setup → dispatches both agents in parallel → runs coverage |

**How it works:**

```
Step 1 — Test Infrastructure Setup  (sequential — fixtures + helpers must exist first)
                    ↓
Step 2a — Unit Test Author  ←─ parallel ─→  Step 2b — Integration Test Author
                    ↓
Step 3 — npm run test:all + coverage report  (sequential — needs both test files)
```

Steps 2a and 2b run **simultaneously** — the orchestrator invokes both specialist agents at the same time, halving the time to generate both test suites.

All three agents are **language-agnostic**: they read `.github/copilot-instructions.md` and `doc/tsd.md` first to detect your stack (Node.js/Jest, Python/pytest, Java/JUnit, C#/xUnit), then generate the correct framework code.

### To Run

1. In Copilot Chat, click the agent selector and choose **Test Orchestrator**
2. Send this single prompt:

```
Read doc/frd.md. Set up ITMS test infrastructure, generate unit + integration tests in parallel, run the suite, and report files below 80% coverage.
```

3. Review the plan — confirm it shows **3 steps** with Step 2 dispatching two agents simultaneously
4. Click **Continue**

> **Tip**: The orchestrator delegates test *writing* to the specialist agents — it never writes test code itself. This is the same pattern as the SDLC Docs Orchestrator in Exercise 02.

> Skip to the [Verify](#verify) section to confirm the output. If you prefer to understand each step individually, continue with the Manual Path below.

---

## Manual Path — Step by Step

---

### Background

Tests are where the FRD's acceptance criteria become code. Each **Given/When/Then** scenario in `doc/frd.md` maps to an automated test. Copilot can read those acceptance criteria and write tests that verify them — turning the FRD into a living test suite.

---

### Step 1 — Generate the Test Configuration

Send this prompt in Copilot Chat (Agent mode):

> **Note**: Adjust the prompt for your stack. If you skipped Exercise 15, remove the context map reference.


Reference `.github/copilot-instructions.md` and, if available, `.github/skills/context-map/context-map.md`.
```
Set up ITMS tests: stack-appropriate framework, tests/unit, tests/integration, tests/fixtures copied from src/data, helpers for task/user factories and JSON-store reset, and scripts test:unit, test:integration, test:all with coverage.
```

---

### Step 2 — Generate Unit Tests for the Service Layer

Send this prompt:

```
Read the service files in src/services/ and the validators in src/validators/.

Generate unit tests in tests/unit/ for the Task Service:

Test Suite: TaskService.createTask()
- Test: Should create a task with status "TO_DO" when all required fields are provided
- Test: Should throw ValidationError when title is missing
- Test: Should throw ValidationError when priority is not LOW/MEDIUM/HIGH
- Test: Should throw NotFoundError when assignedUserId does not exist
- Test: Should set the id field using crypto.randomUUID()

Test Suite: TaskService.getAllTasks()
- Test: Should return all tasks from the repository
- Test: Should return an empty array when no tasks exist
- Test: Should apply status filter when provided

Test Suite: TaskService.getTaskById()
- Test: Should return the matching task when the id exists
- Test: Should throw NotFoundError when no task with the given id exists

Use mocks for the repository layer in all unit tests.
Each test must include Arrange / Act / Assert structure as comments.
```

---

### Step 3 — Generate Integration Tests for the API

Send this prompt:

```
Generate integration tests in tests/integration/ for POST /api/v1/tasks, GET /api/v1/tasks, and GET /api/v1/tasks/:id. Reset the JSON store before each test. Cover valid create with priority HIGH and status TO_DO, missing title 400 VALIDATION_ERROR, unknown assignee 404 NOT_FOUND, list all, filter by status TO_DO, get known task, and get unknown task 404.
```

---

### Step 4 — Run Tests and Check Coverage

> **(Optional) Database stored procedure tests**: If you have completed [Exercise 16](exercise-16-database-sql.md) and are running a real database, ask Copilot to also generate tests in `tests/unit/database/` for the `update_task_status()`, `add_task_dependency()`, and `resolve_task_dependency()` PL/pgSQL functions.

Send this prompt:

```
Run the unit tests using the terminal and show me the coverage report.
Identify any files in src/ that have less than 80% test coverage and list them.
```

---

## Verify

- [ ] Unit tests exist for `TaskService.createTask()`, `getAllTasks()`, and `getTaskById()` with at least 8 test cases total
- [ ] Integration tests cover `POST /api/v1/tasks`, `GET /api/v1/tasks`, and `GET /api/v1/tasks/:id`
- [ ] Tests use the factory helper and JSON store reset helper
- [ ] Each test follows Arrange/Act/Assert structure
- [ ] Coverage report is generated
- [ ] No test requires a running database or live server to pass

---

## Key Takeaway

> When Copilot writes tests from the FRD's Gherkin criteria, the tests become a **living specification**. Every test failure tells you exactly which user story is broken. This is the promise of Behavior-Driven Development — and Copilot makes it practical by eliminating the tedious work of writing the test boilerplate.

---

**Next**: [Exercise 12 — Security Review](exercise-12-security.md)
