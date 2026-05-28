---
name: build-api
description: 'Scaffold and implement the ITMS REST API end-to-end: detect stack from copilot-instructions.md, scaffold entry point and /api/v1 router, load JSON sample data into memory, implement Repository layer, build POST/GET task endpoints with {success,data,error,meta} envelope, and verify all routes with curl. Use when: building ITMS API, scaffolding REST endpoints, setting up in-memory JSON data store, implementing task endpoints, wiring error middleware, verifying API health.'
argument-hint: 'Optionally specify which phase to run: Phase 1 through Phase 6, or omit to run all phases.'
---

# Build API Skill

Implements the ITMS REST API from scratch in six sequential phases. Each phase must be completed and verified before proceeding to the next.

---

## Phase 1 — Detect Stack

1. Read `.github/copilot-instructions.md` in full.
2. Identify and record the following from the instructions file:
   - Language and version (e.g. Python 3.11+)
   - Framework (e.g. FastAPI, Express, Spring Boot, ASP.NET Core)
   - Data strategy (JSON file persistence, no database, no ORM)
   - Test tooling (e.g. pytest + httpx)
3. All subsequent phases must use only the stack identified here — never assume or default to a different language or framework.

---

## Phase 2 — Scaffold

Based on the detected stack:

1. Create the application entry point (e.g. `src/main.py`, `src/index.ts`, `src/main/Application.java`, `Program.cs`).
2. Register a top-level router mounted at `/api/v1/`.
3. Configure JSON body parsing middleware.
4. Register centralised error-handling middleware or exception handler — one global handler only, never inline per-route.
5. Create the folder structure matching the detected stack:
   - Python/FastAPI: `src/main.py`, `src/routers/`, `src/services/`, `src/repositories/`, `src/models/`, `src/middleware/`, `src/exceptions.py`, `src/data/`
   - TypeScript/Node.js: `src/index.ts`, `src/routes/`, `src/services/`, `src/repositories/`, `src/middleware/`, `src/models/`, `src/data/`
   - Java/Spring Boot: `src/main/java/.../controller/`, `service/`, `repository/`, `model/`, `exception/`, `src/main/resources/data/`
   - C#/ASP.NET Core: `Controllers/`, `Services/`, `Repositories/`, `Models/`, `Middleware/`, `Data/`
6. Implement the health check endpoint:
   - Route: GET /api/v1/health
   - Response body: success true, data containing status "ok", timestamp (current ISO-8601), version "1.0.0", error null, meta empty object
   - No authentication required

---

## Phase 3 — Data

1. Copy all four files from `workshop/sample-data/` to the stack-specific data directory:
   - Python → `src/data/`
   - TypeScript → `src/data/`
   - Java → `src/main/resources/data/`
   - C# → `Data/`
   Files: `tasks.json`, `users.json`, `task_dependencies.json`, `task_status_history.json`
2. Implement a startup data-loading mechanism (e.g. lifespan event, module-level singleton, @PostConstruct, IHostedService):
   - Read all four JSON files into memory once on startup
   - Store as in-memory collections (dict/list/map/array)
   - Log a confirmation message at INFO level after successful load
3. Never read from disk on every request — always operate on the in-memory store.
4. Do not use any database driver, ORM, or migration tool.

---

## Phase 4 — Repository

Implement an in-memory repository for Tasks with the following operations. No direct file I/O inside route handlers — all data access goes through this layer only.

1. findAll(filters) — return all tasks; apply optional filters for status, priority, and assignedUserId; return total count alongside the result list for pagination meta.
2. findById(id) — return a single task by UUID; return null or raise a not-found signal if absent.
3. create(data) — generate a new UUID, set createdAt and updatedAt to current timestamp, set status to TO_DO, append to in-memory list, then immediately persist the entire tasks list to `tasks.json` using the stack's synchronous file-write API.
4. updateStatus(id, newStatus) — update the task's status field and updatedAt, set completedAt if newStatus is COMPLETED, append a record to the in-memory task_status_history list and immediately persist `task_status_history.json`, then persist `tasks.json`.

Persistence rule: every mutation must write the full updated list back to the JSON file immediately — no batching, no deferred writes.

---

## Phase 5 — Endpoints

All responses must use the envelope: success (boolean), data (object or array or null), error (null or object with code and message), meta (object, empty or with pagination).

Implement the following three endpoints:

POST /api/v1/tasks
- Validate request body using the stack's schema/model layer (Pydantic, Zod, Bean Validation, DataAnnotations).
- Required field: title (non-empty string).
- Optional fields: description (string), priority (must be one of LOW, MEDIUM, HIGH — reject any other value), assignedUserId (UUID string — must exist in the in-memory users list; if not found return 404 USER_NOT_FOUND).
- Force status to TO_DO regardless of what the client sends.
- On validation failure: HTTP 400, success false, error code VALIDATION_ERROR, list the invalid fields in the message.
- On success: HTTP 201, success true, data is the created task object, meta is empty object.

GET /api/v1/tasks
- Accept optional query parameters: status, priority, assignedUserId, page (default 1), limit (default 10).
- Apply filters via the Repository findAll method.
- Return HTTP 200, success true, data is the paginated array of tasks, meta contains total (total matching tasks), page, and limit.

GET /api/v1/tasks/:id
- Look up task by the id path parameter via Repository findById.
- If not found: HTTP 404, success false, error code TASK_NOT_FOUND.
- If found: HTTP 200, success true, data is the task object, meta is empty object.

---

## Phase 6 — Verify

Start the development server using the stack-appropriate command (e.g. uvicorn, ts-node, mvn spring-boot:run, dotnet run).

Run each of the following curl checks in order. All must pass before the skill is considered complete.

1. GET /api/v1/health — expect HTTP 200 and success true in the response body.
2. GET /api/v1/tasks — expect HTTP 200, success true, and data containing an array.
3. POST /api/v1/tasks with a valid body (title, priority HIGH) — expect HTTP 201, success true, and status field equal to TO_DO in the returned task.
4. GET /api/v1/tasks/:id using the id returned from the previous POST — expect HTTP 200, success true, and data containing the task object.
5. POST /api/v1/tasks with an empty body — expect HTTP 400, success false, and error code VALIDATION_ERROR.

If any check fails, diagnose the failure, apply a fix, restart the server, and re-run that check before continuing.
