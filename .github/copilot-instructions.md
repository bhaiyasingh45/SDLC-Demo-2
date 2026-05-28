# ITMS Project Guidelines

## Project Overview

**Intelligent Task Management System (ITMS)** — a REST API that enables teams to create, assign, and track tasks, manage dependencies, and monitor project progress.

## Language & Framework

- **Language**: Python 3.11+
- **Framework**: FastAPI
- **Validation**: Pydantic v2 models for all request/response schemas
- **Data Store**: JSON file persistence (no database, no ORM, no migrations)
- **Testing**: pytest + httpx (async test client)

## Data Store

- Runtime JSON files live in `src/data/` — copied from `workshop/sample-data/` at project init
- Files: `tasks.json`, `users.json`, `task_dependencies.json`, `task_status_history.json`
- Load all files into memory on application startup (module-level singleton or lifespan event)
- Persist every write with `json.dump` to the corresponding file immediately after mutation
- Never query files on every request — always operate on the in-memory store
- Never use SQLAlchemy, databases, or any DB driver

## API Conventions

- All routes are prefixed `/api/v1/`
- Every response uses this envelope:
  ```json
  { "success": true, "data": {}, "error": null, "meta": {} }
  ```
- On error: `success: false`, `data: null`, `error: { "code": "...", "message": "..." }`, `meta: {}`
- Use `meta` for pagination info: `{ "total": 20, "page": 1, "limit": 10 }`

## Task Domain Model

Fields on every task (match `workshop/sample-data/tasks.json`):
`id` (UUID), `title`, `description`, `priority` (LOW/MEDIUM/HIGH), `status` (TO_DO/IN_PROGRESS/BLOCKED/COMPLETED), `assignedUserId`, `estimatedCompletionDate`, `completedAt`, `createdBy`, `createdAt`, `updatedAt`

Status auto-transitions:
- Set status to `BLOCKED` when any dependency task is not `COMPLETED`
- Set `completedAt` timestamp when status transitions to `COMPLETED`
- Record every status change in `task_status_history.json`

## Error Handling

- Define a single `AppException(HTTPException)` subclass with `code` and `message`
- Register one global exception handler in `main.py` — never handle HTTP errors inline in routes
- Use specific error codes: `TASK_NOT_FOUND`, `USER_NOT_FOUND`, `DEPENDENCY_CYCLE`, `VALIDATION_ERROR`, etc.
- Unhandled exceptions must return HTTP 500 with `code: INTERNAL_ERROR`; never expose stack traces

## Input Validation

- All request bodies must be Pydantic `BaseModel` subclasses — no raw `dict` parameters in route handlers
- Validate UUIDs, enums (priority, status), and date formats at the Pydantic layer
- Reject unknown fields with `model_config = ConfigDict(extra="forbid")`

## Logging

- Use Python's `logging` module with structured output (JSON formatter in production)
- Configure log level via `LOG_LEVEL` environment variable (default: `INFO`)
- Never use `print()` anywhere in the codebase
- Log request method, path, status code, and duration for every request (middleware)
- Log at `ERROR` level for all caught exceptions before returning error responses

## Project Structure

```
src/
  main.py              # FastAPI app, lifespan, global exception handler
  data/                # Runtime JSON files (gitignored, copied from workshop/sample-data/)
  models/              # Pydantic request/response models
  routers/             # One file per resource (tasks.py, users.py, dependencies.py)
  services/            # Business logic — dependency check, status transition, progress summary
  repositories/        # Data access — CRUD on in-memory store + file persistence
  middleware/          # Request logging middleware
  exceptions.py        # AppException and all error codes
tests/
  unit/                # Service and repository tests with mocked store
  integration/         # Full HTTP tests using httpx.AsyncClient + ASGITransport
```

## Testing

- Every service function must have a unit test; mock the repository layer
- Every API endpoint must have at least one integration test (happy path + error path)
- Integration tests use `httpx.AsyncClient` with `app` directly — no running server needed
- Test file naming: `test_<module>.py`
- Run tests: `pytest tests/ -v`

## Code Style

- Follow PEP 8; use `ruff` for linting and formatting
- Use type annotations on all function signatures and return types
- Prefer `async def` for all route handlers and service functions
- Use dependency injection (`Depends`) for repository/service access in routes
- No magic strings — define constants or enums for status values, priority levels, etc.

## Security

- Never log or expose sensitive user data (passwords, tokens) in responses or logs
- Validate and sanitise all user-supplied IDs before using them as dictionary keys
- Do not expose internal file paths or stack traces in API responses

## Documentation

- Add OpenAPI `summary` and `description` to every route decorator
- Export OpenAPI schema: `GET /api/v1/openapi.json` (FastAPI default)
- Keep `README.md` updated with setup steps and `pytest` command

## Commits

Follow Conventional Commits: `feat:`, `fix:`, `test:`, `refactor:`, `docs:`, `chore:`
