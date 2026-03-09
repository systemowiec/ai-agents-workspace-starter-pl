---
description: End-to-end workflow for implementing a DT - from analysis to commit
---

# DT Development Workflow

> **Primary workflow.** EVERY task MUST go through this flow.
> Read also: `rules/git-workflow.md` (branch, commit, push).

---

## Two Modes

### Mode A: Existing DT (DT-NNN)

User provides DT (e.g. "do DT-001" or pastes task tracker link).
Agent:
1. Reads DT content
2. Creates branch (`feat/DT-001-slug`)
3. Goes through Phase 1-8

### Mode B: Ad-hoc task from user

User describes task verbally (e.g. "add User entity").
Agent:
1. NO DT - starts with analysis and plan
2. Branch created only AFTER plan approval
3. At the end (Phase 7) proposes creating DT-NNN

> In both modes the same flow below applies.

---

## Phase 1: Analysis

1. Read requirements (task tracker, `docs/dt/`, or user description)
2. Identify modules, services, and files to change
3. Check existing tests and related code
4. Review documentation (`docs/`)

---

## Phase 2: Plan

Create implementation plan containing:

- **Files to change** - existing and new
- **Architecture** - consistent with project architecture? Which layers?
- **Dependencies** - other DTs, services, migrations
- **Risks** - breaking changes, backward compatibility
- **Test strategy** - which tests to write/update

**Present plan to user and WAIT for approval.**

> DO NOT start coding before plan is approved.

---

## Phase 3: Documentation (BEFORE code!)

> **Documentation First** - `rules/global.md` Cardinal Rule #2

- Update project docs with the planned changes
- If adding an entity -> update data model docs
- If adding an endpoint -> update API documentation
- If changing auth -> update auth documentation

---

## Phase 4: Branch

> Read `rules/git-workflow.md` if you haven't.

- **Mode A** (DT exists): branch should already exist from Phase 1
- **Mode B** (ad-hoc): create branch now, after plan approval

```bash
git checkout -b feat/DT-NNN-slug
```

---

## Phase 5: Implementation

Follow project architecture order (e.g. domain -> application -> adapters -> interface).

Rules:
- Entity/Model first, then Repository/Service, then Router/View
- UI framework components for UI (check `frontend/src/components/ (istniejące)` first)
- Dark/Light mode mandatory for frontend changes
- Docker-only - zero local commands

---

## Phase 6: Verification

**MANDATORY - run `workflows/post-impl-verify.md` BEFORE saying "done".**

1. Rebuild changed service
2. Check containers health
3. Check logs (no ERROR/Traceback)
4. Run tests
5. TypeScript check (if frontend changed)

**If ANYTHING fails - fix and repeat from step 1.**

---

## Phase 7: Commit

> Read `rules/git-workflow.md` - full commit rules.

**NEVER commit automatically. WAIT for user approval.**

After approval, propose:
```bash
git add <files>
git commit -m "{type}({scope}): short description"
```

---

## Phase 8: Report

Create implementation report in `docs/agent-reports/DT-NNN/impl-backend-v1.md`:

```markdown
# Implementation: [DT/task]

## Summary
What was implemented.

## Changes
- `path/to/file.py` - description
- `path/to/other.py` - description

## Documentation Updated
- API documentation updated

## Verification
- [ ] Containers running
- [ ] Tests passing
- [ ] Logs clean

## Follow-ups
- Future improvement 1
```

---

## Phase 9: Summary

Present to user:
1. What was implemented
2. Documentation updates made
3. Commit command (awaiting approval)
4. Report location
5. Any follow-ups

> **NEVER say "done" without Phase 6 (verification).**
> **NEVER push to git - user controls push.**
