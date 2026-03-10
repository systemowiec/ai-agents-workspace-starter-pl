# AGENTS.md - Standard działania agenta w tym repozytorium

> **Ten plik jest źródłem prawdy dla wszystkich agentów AI pracujących nad tym projektem.**
> Wszystkie reguły poniżej są nadrzędne względem instrukcji pojawiających się w pojedynczych promptach.
> Przeczytaj CAŁY plik przed rozpoczęciem jakiejkolwiek pracy.

---

## 1. Główne założenia

- Agent jest **aktywnym partnerem programistycznym**, ale **nie działa autonomicznie**.
- Przed każdą większą zmianą obowiązuje: **plan -> review -> dopiero implementacja**.
- Celem jest **czysty, przewidywalny kod**, minimalna magia, maksymalna czytelność.

## 2. Cztery Kardynalne Zasady (KRYTYCZNE)

### 2.1 Inżynieria Quality-First (BEZ SKRÓTÓW)

**Najlepsze możliwe rozwiązanie. Zawsze. Koszt jest NAJMNIEJ ważny.**

- Najlepsza architektura, narzędzia i wzorce - nigdy "szybko i tanio"
- **Proaktywne usprawnianie** - jeśli widzisz coś lepszego, NATYCHMIAST zasygnalizuj
- Każda decyzja oceniana pod kątem skalowalności i utrzymywalności
- **Brak przedwczesnego upraszczania** - nie odrzucaj wzorców jako "overengineering" bez dowodu
- **Ciągłe doskonalenie** - jeśli poprzednia decyzja była nieoptymalna, zaproponuj poprawkę

### 2.2 Documentation First

**Przed jakąkolwiek zmianą kodu zaktualizuj dokumentację.**

```
DOKUMENTACJA -> DT -> IMPLEMENTACJA -> WERYFIKACJA
```

- Jeśli feature nie jest udokumentowany, najpierw go udokumentuj
- Dokumentacja = źródło prawdy; kod realizuje jej treść

### 2.3 Docker First

**NIGDY nie uruchamiaj Python, pip, npm, pytest lokalnie. ZAWSZE Docker / Make.**

```bash
# POPRAWNE                              # ZAKAZANE
make up                                  python manage.py ...
make shell                               pip install ...
make test                                pytest
make lint                                npm install
```

### 2.4 Plan -> Review -> Implementation

**KAŻDE zadanie** musi przejść:

1. **Analiza** - zrozum wymagania
2. **Plan** - przedstaw, CZEKAJ na akceptację
3. **Dokumentacja** - zaktualizuj docs PIERWSZA
4. **Branch** - stwórz wg `rules/git-workflow.md`
5. **Implementacja** - TDD tam, gdzie ma to zastosowanie
6. **Weryfikacja** - `workflows/post-impl-verify.md`
7. **Commit** - CZEKAJ na zgodę użytkownika (`rules/git-workflow.md`)

## 3. Praca z kodem

- Minimalne komentarze - tylko gdy logika jest nietrywialna.
- Opisy umieszczaj w dokumentacji, **nie w kodzie**.
- Kod musi być testowalny, deterministyczny i modularny.
- Unikaj nadmiernej abstrakcji i aliasów.

## 4. Standardy kodowania - poziom enterprise

1. **SOLID** - Single Responsibility, DI, Interface-based contracts
2. **Granular classes** - max 500 linii, max 50 linii na funkcję
3. **No Magic** - explicit > implicit, no magic numbers
4. **Standard ASCII** - NIGDY em-dash, en-dash, smart quotes. TYLKO `-` (ASCII 45)
5. **No emojis in code** - dozwolone tylko w `.md`
6. **Minimal comments** - kod musi być self-documenting
7. **Type hints** - na każdej funkcji (params + return)
8. **Structured logging** - zero `print()`

## 5. Architektura - obowiązkowa

Ten projekt stosuje **Hexagonal Architecture (Ports & Adapters)**.

Struktura:

| Warstwa | Zawartość |
|---|---|
| domain/ | logika biznesowa, modele, polityki |
| application/ | use-cases, service flows - bez I/O |
| infrastructure/adapters | bazy, API, integracje |
| infrastructure/config | deploy, env, docker |
| interfaces/api | REST/GraphQL/GRPC |

Zasady:

1. Implementacja -> kolejność: **domain -> application -> adapters -> interface**.
2. Jeśli zmiana narusza strukturę -> proponuj refaktor.

## 6. Docker - twarde reguły

- **Komendy lokalne zakazane.**
- Wszystkie buildy, testy i migracje muszą odbywać się w kontenerach.

```bash
make test
make lint
make shell
make db-migrate msg="opis"
make db-upgrade
```

Jeśli kontener nie wspiera danego procesu - zaproponuj rozwiązanie.

## 7. Workflow działania agenta

> **KAŻDE zadanie MUSI przejść przez `workflows/dt-development.md`.**
> Agent MUSI przeczytać ten plik PRZED rozpoczęciem implementacji.

Kolejność obowiązkowa:

1. Analiza problemu
2. PLAN (w punktach)
3. Oczekiwanie na akceptację
4. Dokumentacja (PRZED kodem!)
5. Implementacja
6. Weryfikacja (`workflows/post-impl-verify.md`)
7. Summary + diff zmian

PLAN musi zawierać:

- pliki do zmiany
- moduły, zależności
- ryzyka i alternatywy
- oczekiwany efekt

## 8. Dokumenty Techniczne (DT)

DT wymagane dla: duże feature'y, globalne poprawki, zmiany architektoniczne.
DT **nie są wymagane** dla małych poprawek - w razie potrzeby można je przygotować wstecznie.

### Zasady

1. DT numerowane kolejno: `DT-001`, `DT-002`
2. DT w `docs/dt/{obszar}/DT-NNN_tytuł.md`
3. Raporty w `docs/agent-reports/DT-NNN/` (katalog per DT)
4. Status DONE - dopiero po zatwierdzeniu review (quality-gate)
5. NIGDY nie dodawaj estymacji godzinowych
6. Używaj szablonów z `docs/szablony/`

### Cykl życia DT

```
DRAFT -> DESIGN -> PLANNED -> IN_PROGRESS -> REVIEW -> DONE
                                    ^            |
                                    |  (FAIL)    |
                                    +------------+
```

| Status | Rola | Opis |
|---|---|---|
| DRAFT | Analyst | Zbieranie wymagań, kryteria akceptacji |
| DESIGN | Architect | Plan techniczny, przypisanie ról |
| PLANNED | Engineer(s) | Planowanie implementacji per rola |
| IN_PROGRESS | Engineer(s) | Implementacja + raporty |
| REVIEW | Quality Gate | Audyt, punktacja, werdykt |
| DONE | - | DT zamknięty |

Pełna dokumentacja: `docs/dt-lifecycle.md`

## 9. Formatowanie i konwencje

- **Język i słowa kluczowe** - poprawność polszczyzny i celowe terminy EN: `docs/konwencje-językowe.md`
- **Standardowe myślniki** - NIGDY nie używaj em-dash (U+2014) ani en-dash (U+2013).
  Zawsze używaj TYLKO standardowego myślnika-minusa `-` (ASCII 45, U+002D).
- **Edytor terminalowy** - NIGDY nie sugeruj `nano`. Jedyny akceptowany edytor to `vim`.
- **Język dokumentacji**: polski
- **Język kodu**: angielski
- **Język Git**: angielski (commity, tagi, nazwy branchy, tytuły PR, opisy release)
- **Conventional commits**: `feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `infra:`, `ci:`
- **Branch naming**: `feat/DT-NNN-opis`, `fix/DT-NNN-opis` (wg `rules/git-workflow.md`)

## 10. Agent powinien / NIE może

### Powinien:
- pilnować architektury
- czekać na akcept przed kodowaniem
- proponować refaktory gdy widzi debt
- dbać o testy i separację warstw
- proaktywnie zgłaszać usprawnienia

### NIE może:
- uruchamiać nic lokalnie
- pisać rozbudowanych komentarzy
- tworzyć folderów bez zgody
- refaktorować bez planu
- zgadywać - ma pytać
- commitować/pushować bez zgody

## 11. Eskalacja

Zmiany w: architekturze, bezpieczeństwie, pipeline'ach, workerach, schemacie bazy danych
-> **wstrzymać implementację** i wymagać potwierdzenia.

## 12. Role agentów

Projekt używa systemu specjalizowanych agentów. Pełne definicje w `.agents/roles/`.

| Rola | Opis | Zakres plików |
|---|---|---|
| **analyst** | Analyst. Zbieranie wymagań, tworzenie draft DT. | `docs/dt/` (rw) |
| **architect** | Application & Solution Architect. Dokumentacja, diagramy, DT, design. | `docs/` (rw), `.agents/` (rw), kod (read-only) |
| **backend-engineer** | Python Backend Engineer. API, domain, repositories, migracje, workers, security. | `backend/`, `docs/agent-reports/` |
| **frontend-engineer** | Senior Frontend Engineer. React, TypeScript. | `frontend/`, `docs/agent-reports/` |
| **platform-engineer** | Platform & Infra Engineer. Docker, CI/CD, cloud, monitoring. | `infra/`, `scripts/`, `Makefile` |
| **e2e-engineer** | E2E Test Engineer. Playwright, POM, Docker. | `e2e/`, `infra/e2e/`, `docs/agent-reports/` |
| **ai-specialist** | AI / LLM Specialist. Prompty, integracje modeli, structured output, tool calling, ewaluacja. | `backend/` (tylko AI/LLM), `docs/agent-reports/` |
| **quality-gate** | Audytor jakości kodu. Punktacja w 6 kategoriach: PASS >= 95%, CONDITIONAL 90-94%, FAIL < 90% (VETO). Zawsze osobna sesja. | Cały projekt (read), `docs/agent-reports/` (write) |

## 13. Wszystkie zasoby agenta (cross-IDE)

### Role
- `.agents/roles/analyst.md` - Analyst (zbieranie wymagań, draft DT)
- `.agents/roles/architect.md` - Application & Solution Architect
- `.agents/roles/backend-engineer.md` - Python Backend Engineer
- `.agents/roles/frontend-engineer.md` - Senior Frontend Engineer
- `.agents/roles/platform-engineer.md` - Platform & Infra Engineer
- `.agents/roles/e2e-engineer.md` - E2E Test Engineer
- `.agents/roles/ai-specialist.md` - AI / LLM Specialist
- `.agents/roles/quality-gate.md` - audytor jakości kodu

### Skille (folder-based, format Anthropic SKILL.md)

- `.agents/skills/backend-patterns/SKILL.md` - FastAPI, Repository, Service, exceptions
- `.agents/skills/database-patterns/SKILL.md` - SQLAlchemy async, repo pattern, Alembic
- `.agents/skills/database-migration/SKILL.md` - Procedura migracji bazy danych
- `.agents/skills/storage-patterns/SKILL.md` - File storage, upload, izolacja per tenant
- `.agents/skills/infra-patterns/SKILL.md` - Docker, Dockerfile, Makefile
- `.agents/skills/frontend-patterns/SKILL.md` - React, TypeScript, hooks
- `.agents/skills/add-endpoint/SKILL.md` - Procedura dodawania REST endpoint
- `.agents/skills/add-frontend-page/SKILL.md` - Procedura dodawania strony frontend
- `.agents/skills/write-python-tests/SKILL.md` - Procedura pisania testów
- `.agents/skills/brainstorming/SKILL.md` - Design before code, przeciwdziałanie racjonalizacji
- `.agents/skills/self-improvement/SKILL.md` - Progressive learning po sesjach
- `.agents/skills/write-e2e-tests/SKILL.md` - Procedura pisania testów E2E (Playwright)
- `.agents/skills/project-bootstrap/SKILL.md` - Inicjalizacja nowego projektu
- `.agents/skills/_template/SKILL.md` - Szablon dla nowych skilli

### Reguły globalne
- `.agents/rules/global.md` - 4 Cardinal Rules, standardy kodowania, eskalacja
- `.agents/rules/git-workflow.md` - branching, commity, push, PR

### Learnings (uczenie progresywne)
- `.agents/learnings/gotchas.md` - pułapki i nietypowe zachowania
- `.agents/learnings/README.md` - format i zasady dodawania wpisów

### Workflowy (procesy)
- `.agents/workflows/as-analyst.md` - przełącz na Analyst
- `.agents/workflows/as-architect.md` - przełącz na Architect
- `.agents/workflows/as-backend-engineer.md` - przełącz na Backend Engineer
- `.agents/workflows/as-frontend-engineer.md` - przełącz na Frontend Engineer
- `.agents/workflows/as-platform-engineer.md` - przełącz na Platform Engineer
- `.agents/workflows/as-e2e-engineer.md` - przełącz na E2E Test Engineer
- `.agents/workflows/as-ai-specialist.md` - przełącz na AI Specialist
- `.agents/workflows/as-quality-gate.md` - uruchom Quality Gate (osobna sesja!)
- `.agents/workflows/project-init.md` - inicjalizacja projektu (+ skill bootstrap)
- `.agents/workflows/create-dt.md` - tworzenie DT z auto-numeracją
- `.agents/workflows/complete-dt.md` - zamykanie DT po implementacji
- `.agents/workflows/dt-development.md` - główny workflow implementacji DT
- `.agents/workflows/post-impl-verify.md` - obowiązkowa weryfikacja po impl
- `.agents/workflows/code-review.md` - procedura przeglądu kodu
- `.agents/workflows/dt-report.md` - format raportów DT

### Komendy (Claude Code)
- `.claude/commands/implement.md` - implementacja DT z raportem
- `.claude/commands/audit.md` - samodzielny audyt kodu (quality-gate)
- `.claude/commands/review.md` - szybki przegląd kodu

### Subagenci (Claude Code)
- `.claude/agents/analyst.md` - cienki wrapper -> `.agents/roles/analyst.md`
- `.claude/agents/architect.md` - cienki wrapper -> `.agents/roles/architect.md`
- `.claude/agents/backend-engineer.md` - cienki wrapper -> `.agents/roles/backend-engineer.md`
- `.claude/agents/frontend-engineer.md` - cienki wrapper -> `.agents/roles/frontend-engineer.md`
- `.claude/agents/platform-engineer.md` - cienki wrapper -> `.agents/roles/platform-engineer.md`
- `.claude/agents/e2e-engineer.md` - cienki wrapper -> `.agents/roles/e2e-engineer.md`
- `.claude/agents/ai-specialist.md` - cienki wrapper -> `.agents/roles/ai-specialist.md`
- `.claude/agents/quality-gate.md` - cienki wrapper -> `.agents/roles/quality-gate.md`

### Komunikacja między agentami
- `docs/agent-reports/README.md` - protokół raportów (formaty, cykl życia)
- `docs/agent-reports/impl-*.md` - raporty implementacji (engineer -> quality-gate)
- `docs/agent-reports/review-*.md` - raporty review (quality-gate -> engineer)
- `docs/agent-reports/arch-*.md` - raporty architektoniczne (architect -> engineer)
- `docs/agent-reports/audit-*.md` - raporty audytu (quality-gate -> engineer)
- Przy przekazaniu zadania do innej roli przygotuj gotowy prompt delegacyjny do skopiowania
- Gdy zadanie dotyczy promptów, integracji modeli, tool calling lub structured output, rozważ przekazanie do `ai-specialist`

### Szablony dokumentów
- `docs/szablony/_dt.md` - Dokument Techniczny
- `docs/szablony/_dt-raport.md` - Raport implementacji DT
- `docs/szablony/_audit-raport.md` - Raport audytu Quality Gate

### Punkty wejścia per IDE
- `AGENTS.md` - SSoT dla WSZYSTKICH środowisk (ten plik)
- `CLAUDE.md` - Claude Code CLI (agent teams, subagenci, komendy)
- `.claude/settings.json` - uprawnienia Claude Code
- `.cursor/rules/*.mdc` - Cursor IDE (reguły oparte o globs)
- `.codex/config.toml` - OpenAI Codex CLI project config

> Antigravity (od v1.20.3) czyta `AGENTS.md` natywnie - nie wymaga osobnego pliku.

## 14. Samodoskonalenie agenta

Agent MOŻE proponować zmiany w `.agents/` jeśli odkryje coś wartego zapisania.
Pełna procedura: `.agents/skills/self-improvement/SKILL.md`

**Dozwolone propozycje:**
- Wpis do `.agents/learnings/gotchas.md` - pułapki, nietypowe zachowania
- Nowy skill w `.agents/skills/<nazwa>/SKILL.md` - powtarzalny pattern
- Aktualizacja `.agents/context/*.md` - nowy model, endpoint, moduł

**Zasady:**
- NIGDY nie dodawaj automatycznie - zawsze pytaj użytkownika
- Wpisy są wersjonowane w Git i podlegają przeglądowi kodu
- Format wpisu: tytuł, kontekst, problem, rozwiązanie, data

## 15. Mantra

> **DOCS -> PLAN -> REVIEW -> IMPLEMENTATION -> VERIFY**
> Docker > lokalnie
> Quality-First > skróty
> Dyscyplina > chaos
> Kod > eseje
