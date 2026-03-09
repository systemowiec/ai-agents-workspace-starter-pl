---
description: Creating DTs and implementation reports
---

# DT Report Workflow

> Procedure for creating DTs and writing implementation reports.

---

## When to Use

- After implementing a task (Phase 8 of `dt-development.md`)
- When proposing a new DT for ad-hoc work

---

## 1. DT Format (Markdown)

```markdown
# DT-NNN: Title

## Goal
Short description of the objective.

## Scope
- [ ] Task 1
- [ ] Task 2

## Acceptance Criteria
- Criterion 1
- Criterion 2

## Dependencies
- DT-YYY (if any)

## Technical Notes
Implementation details.
```

## 2. Next DT Number

Scan existing DTs to determine the next number:

```bash
ls docs/dt/*/DT-*.md 2>/dev/null | sort -t- -k2 -n | tail -1
```

## 3. Implementation Report

After completing a DT, create report in `docs/agent-reports/`:

```markdown
# DT-NNN: Title - Implementation Report

## Summary
What was implemented.

## Changes
- `backend/app/domain/entities.py` - new entity
- `backend/app/application/service.py` - business logic
- `backend/app/tests/test_service.py` - unit tests

## Documentation Updated
- API documentation updated
- `docs/dt/` - updated diagram

## Verification
- [ ] Containers running
- [ ] Tests passing
- [ ] Logs clean

## Follow-ups
- DT-YYY: Add related feature
```

---

## Rules

- **NEVER** write time estimates or AI mentions in DTs
- **NEVER** abbreviate or summarize reports
- Reports in `docs/agent-reports/DT-NNN/` follow naming: `impl-{rola}-v{N}.md`
