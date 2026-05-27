# Exercise 16 — Database Design, SQL Scripts & PL/SQL

**Duration**: 5 minutes  
**Copilot Feature**: Local Agent + Instructions  
**Goal**: Generate the full database schema, migration scripts, seed data, and stored procedures for the ITMS application.

---

> ---
> 🟡 **OPTIONAL EXERCISE**
>
> This exercise is **not required** to complete the mandatory track. It generates the full SQL schema, migrations, and stored procedures. Complete it if database-layer code generation is relevant to your role, or revisit it after the workshop.
>
> **Best after**: Exercise 15 (or Exercise 08 if skipping Ex 15) &nbsp;|&nbsp; **Return to mandatory track**: [Exercise 11 — Write Tests](exercise-11-testing.md)
> ---


## Background

Most business applications have critical business logic in the database layer — task dependency validation, transactional status updates, audit triggers. This exercise generates the complete SQL layer for ITMS, including PL/SQL (Oracle) or PL/pgSQL (PostgreSQL) stored procedures.

> **If you don't have a database available**, follow the Mock Data path at the end of this exercise — Copilot will create JSON mock data with the same schema structure so your API can run without a real DB.

---

## Step 1 — Generate the Database Schema

In Copilot Chat (Agent mode), send:

```
Using doc/tsd.md ER diagram and optional context map, create PostgreSQL migrations in db/migrations/: users, tasks, task_dependencies, task_status_history, audit_log. Include required enums, FKs with ON DELETE RESTRICT, uniqueness/no-self dependency constraint, JSONB audit values, and indexes on assigned_user_id+status, dependency task IDs, status+priority.
```

---

## Step 2 — Create Stored Procedures / Functions

Send this prompt:

```
Create PL/pgSQL files for create_task, update_task_status, add_task_dependency, resolve_task_dependency, and an audit trigger. Enforce enum/user/dependency validation, transactional status/history updates, BLOCKED logic for incomplete dependencies, unblock when resolved, and audit INSERT/UPDATE/DELETE on tasks and task_dependencies.
```

---

## Step 3 — Create Seed Data

Send this prompt:

```
Create db/seeds/001_seed_data.sql with 10 realistic users, 20 self-consistent tasks across LOW/MEDIUM/HIGH and TO_DO/IN_PROGRESS/BLOCKED/COMPLETED, 5 dependencies, and matching status history.
```

---

## Step 4 — Mock JSON Path (No Database Available)

> Skip this step if you have a working database.

If you don't have PostgreSQL available, send this prompt instead:

```
No database is available. Create db/mock JSON files matching the schema (users, tasks, task_dependencies, task_status_history), db/mock/README.md, and src/repositories/mock implementations that read the mock JSON data.
```

---

## Verify

Check `db/` folder:

- [ ] Migration files are numbered and sequential (001_, 002_…)
- [ ] Each migration creates a table with all required columns
- [ ] Indexes are defined
- [ ] Stored procedures include transaction handling and exception raising
- [ ] Dependency blocking logic correctly sets task status to BLOCKED
- [ ] Audit trigger is created for the correct tables
- [ ] Seed data is self-consistent

---

## Key Takeaway

> Copilot understands both SQL DDL and procedural SQL (PL/pgSQL/PL/SQL). By anchoring the prompt to the TSD's ER diagram and the business rules from the FRD, you get database code that actually matches your requirements — not generic boilerplate. The stored procedures encode business rules (dependency validation, status transitions) in the database layer where they belong.

---

**Return to Mandatory Track →**: [Exercise 11 — Write Tests](exercise-11-testing.md)
