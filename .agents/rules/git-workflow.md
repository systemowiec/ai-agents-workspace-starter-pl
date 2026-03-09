---
trigger: always_on
---

# Git Workflow - zasady pracy z repozytorium

> **Ten plik opisuje jak agent pracuje z Git w tym projekcie.**
> Dotyczy branchowania, commitowania i pushowania.

## Branch

### Tworzenie brancha

Każda większa zmiana wymaga osobnego brancha:

```
{type}/{TICKET-ID}-short-description
```

Przykłady:
```bash
feat/DT-001-user-entity
fix/DT-012-jwt-ttl-increase
refactor/DT-031-hexagonal-structure
docs/DT-040-architecture
```

Typy: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`

### Kiedy branch jest wymagany

- Nowa funkcjonalność (endpoint, strona, model) - **ZAWSZE**
- Bugfix - **ZAWSZE**
- Drobna poprawka w dokumentacji - można na main (po uzgodnieniu z userem)

## Commit

### Format wiadomości

```
{type}({scope}): krótki opis

- zmiana 1
- zmiana 2
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
- **NIGDY** nie wpisuj estymacji czasowych ani wzmianek o AI
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
