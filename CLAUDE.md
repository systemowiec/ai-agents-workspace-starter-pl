# [PROJECT_NAME]

> Przeczytaj `AGENTS.md` - pełne reguły projektu (źródło prawdy).
> Ten plik jest ładowany automatycznie przez Claude Code (w tym przez każdego teammate'a w Agent Teams).

## Architektura

Hexagonal Architecture (Ports & Adapters).
Kolejność implementacji: domain -> application -> adapters -> interface.

## Granice plików (Agent Teams)

Każdy teammate MUSI przestrzegać granic plików. Edycja poza swoim zakresem = błąd.

| Teammate | Może zapisywać | NIE może dotykać |
|---|---|---|
| backend-engineer | backend/, docs/agent-reports/ | frontend/, infra/, terraform/, .agents/roles/ |
| quality-gate | docs/agent-reports/ | NICZEGO INNEGO (readonly) |
| architect | docs/, .agents/ | backend/, frontend/, infra/ |
| analyst | docs/dt/ | backend/, frontend/, infra/, e2e/, .agents/roles/ |
| platform-engineer | infra/, terraform/, scripts/, Makefile, docs/agent-reports/ | backend/app/, frontend/src/ |
| frontend-engineer | frontend/, docs/agent-reports/ | backend/, infra/, terraform/ |
| e2e-engineer | e2e/, infra/e2e/, docs/agent-reports/ | backend/app/, frontend/src/ |

Dwóch teammate'ów nie może jednocześnie edytować tego samego pliku.
Jeśli musisz zmienić plik innego teammate'a - wyślij mu wiadomość.

## Subagenci

Definicje subagentów w `.claude/agents/`. Każdy odwołuje się do pełnej roli z `.agents/roles/`.

| Subagent | Plik roli | Narzędzia |
|---|---|---|
| backend-engineer | `.agents/roles/backend-engineer.md` | Read, Write, Edit, Bash, Glob, Grep |
| quality-gate | `.agents/roles/quality-gate.md` | Read, Grep, Glob (readonly) |
| architect | `.agents/roles/architect.md` | Read, Write, Edit, Glob, Grep |
| analyst | `.agents/roles/analyst.md` | Read, Write, Edit, Glob, Grep |
| platform-engineer | `.agents/roles/platform-engineer.md` | Read, Write, Edit, Bash, Glob, Grep |
| frontend-engineer | `.agents/roles/frontend-engineer.md` | Read, Write, Edit, Bash, Glob, Grep |
| e2e-engineer | `.agents/roles/e2e-engineer.md` | Read, Write, Edit, Bash, Glob, Grep |

## Komendy

| Komenda | Opis |
|---|---|
| /implement | Implementacja DT z raportem |
| /audit | Pełny audyt kodu (quality-gate) |
| /review | Szybki przegląd kodu |

## Skille

Skille w `.agents/skills/<nazwa>/SKILL.md` (folder-based, format Anthropic).
Ładowane automatycznie przez Claude Code (przez symlink `.claude/skills`).

## Komunikacja między agentami

### Raporty (file-based, przetrwają zamknięcie sesji)

| Typ raportu | Plik | Kto zapisuje | Kto czyta |
|---|---|---|---|
| Implementacja | `docs/agent-reports/DT-NNN/impl-{rola}-v1.md` | backend-engineer | quality-gate, architect |
| Review | `docs/agent-reports/DT-NNN/review-v1.md` | quality-gate | backend-engineer |
| Architektura | `docs/agent-reports/DT-NNN/arch.md` | architect | backend-engineer |

### Wiadomości (Agent Teams)

WYŚLIJ wiadomość gdy:
- Skończyłeś swój task -> napisz do następnej osoby w kolejce
- Zmienił się API contract -> napisz do `frontend-engineer`
- Znalazłeś blocker -> napisz do leada

NIE wysyłaj broadcastu bez potrzeby - każda wiadomość zużywa kontekst.
Ważne decyzje zapisuj w plikach (przetrwają compaction).

### Cykl implementacyjny

```
Architect: analiza -> arch-*.md -> wiadomość do `backend-engineer` z wytycznymi
Engineer: implementacja -> make lint && make test -> impl-*.md -> wiadomość do `quality-gate`
Quality Gate: review -> review-*.md
  PASS/CONDITIONAL -> wiadomość do leada "DT done"
  FAIL -> wiadomość do `backend-engineer` z listą poprawek -> engineer poprawia -> nowy cykl
```

## Docker First

Zakazane komendy lokalne: python3, pip, npm, npx, pytest, ruff, mypy, alembic
Dozwolone: make test, make lint, make build, make up, make shell

## Weryfikacja

Po każdej implementacji uruchom: `make lint && make test`
Przed zamknięciem sesji upewnij się, że raporty są zapisane w `docs/agent-reports/`.
