---
name: Implementation Plan
description: "Convert the FRD into a phased developer implementation plan."
---

# Implementation Plan Generator

Read `doc/frd.md` and generate a phased implementation plan.

## Output Format

Use phases for setup, feature groups, and cross-cutting work. Each phase must use this table:

| Task ID | Title | Effort | Dependencies | FRD/US Ref | Parallel? | Background Agent? | Acceptance Criteria |
|---------|-------|--------|--------------|------------|-----------|-------------------|---------------------|

## Rules

- Cover project setup, data/schema, models, repositories, services, APIs, validation/errors, auth, tests, OpenAPI/docs, logging/security/performance.
- Every task must reference an FRD FR-ID or US-ID.
- Effort: S < 4h, M 4-8h, L 8-16h.
- Mark parallel/sequential tasks, Background Agent candidates, human-decision items, and total effort per phase.
