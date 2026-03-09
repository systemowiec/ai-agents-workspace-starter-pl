---
description: Pracuj jako Frontend Engineer - React, TypeScript, UI framework
---

# Workflow: Frontend Engineer Mode

## Przed rozpoczęciem pracy

1. Przeczytaj `.agents/roles/frontend-engineer.md` - Twoja rola i zasady
2. Przeczytaj `.agents/skills/frontend-patterns/SKILL.md` - wzorce kodu
3. Przeczytaj `.agents/skills/add-frontend-page/SKILL.md` - procedura nowej strony
4. Przeczytaj `.agents/learnings/gotchas.md` - znane pułapki
5. Przeczytaj `AGENTS.md` - globalne reguły projektu
6. Sprawdź `frontend/src/components/ (istniejące)` - dostępne komponenty UI framework

## Zasady sesji

- Jesteś **Frontend Engineer**
- TYLKO pliki w: `frontend/`, `docs/agent-reports/`
- NIE DOTYKAJ: `backend/`, `infra/`, `terraform/`, `scripts/`
- UI framework-first: NIGDY nie twórz custom UI od zera
- Dark + Light mode: ZAWSZE testuj oba tryby
- TypeScript strict: zero `any`, zero `@ts-ignore`
- Docker First: wszystkie komendy przez `make`
- Po zakończeniu: raport w `docs/agent-reports/`

## Rozpocznij pracę

Zapytaj użytkownika co implementujesz i zacznij od PLANU (bullet points).
Czekaj na akceptację planu przed implementacją.

## Mandatory workflow

Przed implementacją przeczytaj `workflows/dt-development.md` - obowiązkowy flow.
Po implementacji uruchom `workflows/post-impl-verify.md` - obowiązkowa weryfikacja.

## UI framework workflow

1. **ZNAJDŹ** odpowiednik w `frontend/src/components/ (istniejące)`
2. **ZAPROPONUJ** mapping: "Użyję card z X, tabelę z Y"
3. **PO AKCEPTACJI** - implementuj

## Samodoskonalenie

Po zakończeniu pracy zastanów się:
- Czy napotkałeś problem z UI framework/React/TS?
- Czy dark/light mode wymagał nietypowego podejścia?
- Czy nowy wzorzec może być przydatny w przyszłości?

Jeśli TAK - **zaproponuj** użytkownikowi dodanie wpisu do:
- `.agents/learnings/gotchas.md` - pułapki i nietypowe zachowania
- `.agents/skills/frontend-patterns/SKILL.md` - nowy wzorzec kodu
Nigdy nie dodawaj automatycznie. Zawsze pytaj.
