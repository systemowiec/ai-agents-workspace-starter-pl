---
description: Mandatory verification after every code change - smoke tests, unit tests, container logs. Run BEFORE telling user "done".
---

# Post-Implementation Verification

> **MANDATORY after EVERY code change - backend, frontend, config, Docker, routing, infrastructure.**
> Agent CANNOT report "done" without completing ALL steps below.
> If any step fails - fix and repeat from start.

---

## When to Run

**ALWAYS** - after completing implementation of every feature, bugfix, refactor, config change.
Applies to both backend and frontend changes.

---

## Step 1: Identify Scope

| Change in...                    | Services to verify        |
| ------------------------------- | ------------------------- |
| `backend/`                      | backend container         |
| `frontend/`                     | frontend container        |
| `infra/`                        | ALL containers            |
| `docker-compose*.yml`           | ALL containers            |
| migrations                      | backend container (DB)    |

---

## Step 2: Rebuild Changed Service

```bash
make build
make up
```

Wait 5-10 seconds for startup, then proceed to step 3.

---

## Step 3: Container Health Check

```bash
make status
```

**REQUIRED:** All services `Up (healthy)` or `Up`. None `Exited`.
If anything is `Exited` - read logs (Step 4), fix, return to Step 2.

---

## Step 4: Log Check (no ERROR/EXCEPTION)

```bash
make logs
```

**LOOK FOR:** `ERROR`, `Exception`, `Traceback`, `Fatal`, `CRITICAL`.
If anything found - fix and return to Step 2.

---

## Step 5: Smoke Test - Key Endpoints

```bash
# Backend health check
docker compose -f infra/docker-compose.yml exec backend curl -sf http://localhost:8000/health | head -c 200
```

---

## Step 6: Run Tests

### Backend:
```bash
make test
```

### Frontend (if changed):
```bash
make lint
```

### Migration check (if model changed):
```bash
make db-upgrade
```

**REQUIRED:** No test FAIL. If fail -> fix and repeat.

---

## Step 7: Verification Summary

Before telling user "done", confirm:

| Question                                    | Required Answer |
| ------------------------------------------- | --------------- |
| Are containers running? (`make status`)     | All `Up`        |
| Are logs clean? (no ERROR/Fatal)            | Yes             |
| Do smoke tests pass?                        | Yes             |
| Do unit tests pass?                         | Yes (or N/A)    |
| Does TypeScript compile? (if frontend)      | Yes (or N/A)    |

**If ANY "No" -> DO NOT report done. Fix and repeat cycle.**

---

## ABSOLUTE RULE

> **Agent NEVER tells user "done", "implemented", "works" without completing this workflow.**
> This is equivalent to "shipping to production without tests" - unacceptable.
