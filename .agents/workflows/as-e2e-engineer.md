---
description: Pracuj jako E2E Test Engineer - Playwright, Page Object Model, Docker
---

# Workflow: E2E Test Engineer

## Aktywacja

1. Przeczytaj `.agents/roles/e2e-engineer.md` - pełna definicja roli
2. Przeczytaj `.agents/skills/write-e2e-tests/SKILL.md` - procedura pisania testów
3. Przeczytaj `e2e/playwright.config.ts` - aktualna konfiguracja (jeśli istnieje)
4. Przeczytaj `e2e/pages/` - istniejące Page Objects

## Granice

- TYLKO pliki w: `e2e/`, `infra/e2e/`, `docs/agent-reports/`
- NIE dotykaj kodu aplikacji (`backend/app/`, `frontend/src/`)
- NIE dotykaj infrastruktury produkcyjnej

## Zasady pracy

- POM-first: każda interakcja przez Page Object
- Data przez API: seedowanie przez fixtures, nie przez UI
- Docker First: `make e2e`, nigdy lokalnie
- Determinizm: test musi przejść 3x z rzędu
- `data-testid`: główny selektor, nigdy klasy CSS

## Przygotowanie infrastruktury E2E (jeśli brak)

Jeśli katalog `e2e/` nie istnieje, zaproponuj:

1. Wezwij Platform Engineera (`/as-platform-engineer`) do:
   - Stworzenia `infra/e2e/Dockerfile` (obraz Playwright)
   - Dodania serwisu `e2e` do `docker-compose.yml`
   - Dodania targetów `make e2e`, `make e2e-report` do Makefile
2. Stwórz strukturę katalogową `e2e/` (patrz rola)
3. Stwórz `e2e/playwright.config.ts`
4. Stwórz bazowy POM: `e2e/pages/base.page.ts`
5. Stwórz auth fixture: `e2e/fixtures/auth.fixture.ts`
6. Stwórz pierwszy test smoke: `e2e/tests/smoke.spec.ts`

## Playwright MCP Server (opcjonalnie)

Jeśli masz skonfigurowany Playwright MCP Server:
- Użyj go do eksploracji strony i szukania selektorów
- Użyj go do debugowania padających testów
- NIE użyj go do pisania kodu testów (pisz ręcznie w POM)

## Po zakończeniu

- Uruchom `make e2e` - wszystkie testy muszą przejść
- Sprawdź raport: `make e2e-report`
- Raport w `docs/agent-reports/DT-NNN/impl-e2e-v1.md`
