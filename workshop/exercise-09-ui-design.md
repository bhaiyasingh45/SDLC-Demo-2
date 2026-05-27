# Exercise 09 — Design & Scaffold the Task Management UI

| | |
|---|---|
| **Duration** | 5 minutes |
| **Feature** | Local Agent — UI Scaffolding from API Spec |
| **Goal** | Use Copilot to scaffold a frontend for the ITMS that lists tasks and shows individual task details |

---

## Background

Your ITMS API is now running. This exercise builds the read-facing UI that team members use to browse and inspect tasks. Copilot reads the **FRD user stories** and the **running API routes** to scaffold components already wired to real endpoints.

The UI covers two views:
- **Task List** — a table of all tasks from `GET /api/v1/tasks`
- **Task Detail** — full task information from `GET /api/v1/tasks/:id`

---

## Step 1 — Scaffold the UI Project

In Copilot Chat (Agent mode, Local agent), send:

```
Read src/routes/v1/tasks.ts and src/routes/v1/users.ts. Scaffold ui/ with React + TypeScript + Vite, folders components/pages/services/types, and ui/.env.example. Create ui/src/types/task.types.ts with TaskStatus='TO_DO'|'IN_PROGRESS'|'BLOCKED'|'COMPLETED', TaskPriority='LOW'|'MEDIUM'|'HIGH', Task, User, TaskStatusHistory, TaskDependency, and ApiEnvelope<T>. Create ui/src/services/api.ts using VITE_API_BASE_URL default http://localhost:3000/api/v1 and functions: listTasks(filters?), getTask(id), listUsers(), getTaskHistory(id), getTaskDependencies(id), updateTaskStatus(id, body). Return unwrapped envelope data. No pages yet.
```

---

## Step 2 — Generate the Task List Page

The Task List maps to **UC-005 (List Tasks)** in `doc/frd.md`. Send:

```
Read ui/src/services/api.ts and ui/src/types/task.types.ts. Create TaskListPage as `/`: load listTasks() + listUsers() in parallel; show SummaryBar counts; filters for status/priority/assignee that re-query; table columns short ID, title, priority, StatusBadge, assigned full name, due date; row click -> /tasks/:id. Extract StatusBadge with the existing green/blue/red/grey status colors and SummaryBar. Keep components under 50 lines.
```

---

## Step 3 — Generate the Task Detail Page

Task Detail maps to **UC-004 (Task Detail)** in `doc/frd.md`. Send:

```
Read ui/src/services/api.ts and ui/src/types/task.types.ts. Create TaskDetailPage at /tasks/:id. Fetch task, history, dependencies, users in parallel; resolve names. Sections: task info with StatusBadge; dependencies table with each dependency status and blocked warning if any dependency is not COMPLETED; status history newest first; update-status form using first user as changedBy, refresh on success. Add Back to Task List.
```

---

## Step 4 — Wire Router, Apply Visual Design, and Run the UI

Once both pages are scaffolded, send:

```
Wire BrowserRouter: / -> TaskListPage, /tasks/:id -> TaskDetailPage, catch-all -> /. Install react-router-dom, run Vite on port 5173, and verify list, filters, resolved user names, detail navigation, dependencies warning, history, and update-status refresh.
```

If any step fails, diagnose the error and apply the fix before moving on.

> **Both servers must be running**: the API on port 3000 (Exercise 08) and the UI dev server on port 5173.

---

## Checkpoint — UI Complete ✅

At the end of this exercise you should have:

```
ui/
├── src/
│   ├── components/
│   │   └── StatusBadge.tsx
│   ├── pages/
│   │   ├── TaskListPage.tsx
│   │   └── TaskDetailPage.tsx
│   ├── services/
│   │   └── api.ts
│   └── types/
│       └── task.types.ts
└── .env.example
```

Each page traces back to a `UC-ID` in `doc/frd.md` — the same document that drove the API and the tests.

---

## Key Takeaway

> The same document-driven workflow — **FRD → architecture → prompt → Copilot generates** — works equally well for UI as it did for the API. You did not write a single component from scratch; you directed Copilot by referencing the FRD use cases and the exact API routes already in your workspace.

---

**Next**: [Exercise 10 — Complete ITMS: CLI with /plan & /fleet + Coding Agent via MCP](exercise-10-cli-fleet-and-coding-agent.md)