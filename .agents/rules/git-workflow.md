---
trigger: always_on
---

# Git Workflow - zasady pracy z repozytorium

> **Ten plik opisuje jak agent pracuje z Git w tym projekcie.**
> Dotyczy branchowania, commitowania i pushowania.

## Branch

### Tworzenie brancha

Każda większa zmiana wymaga osobnego brancha.

**Format z DT (preferowany):**
```
{type}/{TICKET-ID}-short-description
```

**Format ad-hoc (bez DT):**
```
{type}/short-description
```

Przykłady:
```bash
feat/DT-001-user-entity
fix/DT-012-jwt-ttl-increase
refactor/DT-031-hexagonal-structure
docs/DT-040-architecture
chore/update-dependencies
test/add-missing-unit-tests
```

### Reguły nazewnictwa

- **Typy**: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`
- **Tylko lowercase** - nigdy wielkie litery (poza ID: `DT-NNN`)
- **Dozwolone znaki**: `[a-z0-9-/]` + uppercase w ID
- **Separator**: `-` (myślnik) między słowami
- **Język opisu**: angielski
- **Zwięzłość**: 2-4 słowa opisu, max 50 znaków całej nazwy

### Kiedy branch jest wymagany

- Nowa funkcjonalność (endpoint, strona, model) - **ZAWSZE**
- Bugfix - **ZAWSZE**
- Drobna poprawka w dokumentacji - można na main (po uzgodnieniu z userem)

## Commit

### Format wiadomości

> **Język: angielski.** Dotyczy WSZYSTKICH operacji Git: commity, tagi, nazwy branchy, tytuły PR, opisy release.

```
{type}({scope}): short description in English

- change 1
- change 2
```

Przykłady:
```bash
feat(api): add user registration endpoint
fix(auth): correct JWT token refresh logic
docs(arch): update data model with new entities
refactor(domain): extract payment service
infra: add Redis healthcheck
```

### Zasady commitowania

> **NIGDY nie commituj automatycznie.**
> **ZAWSZE pytaj użytkownika o zgodę przed commitem.**
> Użytkownik może chcieć zebrać więcej zmian do jednego commita.
>
> Czekaj na jawną zgodę: "commituj", "ok możesz commitować", "zrób commita" itp.

- Max 1 linia tytułu + opcjonalnie 3-5 bulletów
- **Język commitów: WYŁĄCZNIE angielski.** Nigdy po polsku.
- **NIGDY** nie wpisuj estymacji czasowych ani wzmianek o AI
- **NIGDY** nie wspominaj nazw narzędzi AI (Cursor, Claude, Copilot, Antigravity, itp.) jako autorów zmian - ani w commitach, ani w kodzie, ani w raportach
- **NIGDY** nie commituj: `.env`, `node_modules/`, `storage/`, kluczy SSH/secrets

## Push

> **NIGDY nie pushuj.** Push jest wyłącznie w gestii użytkownika.
> Agent nie ma prawa używać `git push`.

## Pull Request

- **NIGDY** nie pushuj bezpośrednio na `main`
- Zawsze Pull Request
- Agent nie tworzy PR samodzielnie - informuje użytkownika, że zmiany są gotowe do PR

## Git komendy

> **NIGDY** nie uruchamiaj komend `git` destrukcyjnych bez zgody użytkownika.
> Proponuj komendy do skopiowania.

```bash
git checkout -b feat/DT-001-user-entity
git add backend/app/domain/entities.py backend/app/application/user_service.py
git commit -m "feat(api): add user entity and service"
```

---

## See also (MANDATORY)

- **Workflow** - read `workflows/dt-development.md` (full flow)
- **DT report** - read `workflows/dt-report.md` (reports)
