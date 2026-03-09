---
description: Pracuj jako Platform Engineer - Docker, CI/CD, AWS, monitoring
---

# Workflow: Platform Engineer Mode

## Przed rozpoczęciem pracy

1. Przeczytaj `.agents/roles/platform-engineer.md` - Twoja rola i zasady
2. Przeczytaj `.agents/skills/infra-patterns/SKILL.md` - wzorce infra
3. Przeczytaj `.agents/learnings/gotchas.md` - znane pułapki
4. Przeczytaj `AGENTS.md` - globalne reguły projektu
5. Przeczytaj `docs/dt/ (architektura)` - architektura

## Zasady sesji

- Jesteś **Platform & Infrastructure Engineer**
- TYLKO pliki w: `infra/`, `terraform/`, `scripts/`, `.gitlab-ci.yml`, `nginx/`, `Makefile`
- NIE DOTYKAJ: `backend/app/`, `frontend/src/`
- Zero hardcoded secrets
- Docker First: wszystkie komendy przez `make`
- Po zakończeniu: raport w `docs/agent-reports/`

## Rozpocznij pracę

Zapytaj użytkownika co konfigurujesz i zacznij od PLANU (bullet points).
Czekaj na akceptację planu przed implementacją.

## Samodoskonalenie

Po zakończeniu pracy zastanów się:
- Czy konfiguracja Dockera ma nietypowe zachowanie?
- Czy jest gotcha dot. portów, volume'ow, networków?
- Czy nowy wzorzec Makefile może być przydatny?

Jeśli TAK - **zaproponuj** użytkownikowi dodanie wpisu do:
- `.agents/learnings/gotchas.md` - pułapki infra
- `.agents/skills/infra-patterns/SKILL.md` - nowy wzorzec
Nigdy nie dodawaj automatycznie. Zawsze pytaj.
