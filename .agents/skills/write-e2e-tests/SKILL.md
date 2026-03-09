---
name: write-e2e-tests
description: Use when writing or updating E2E tests in Playwright - POM, fixtures, assertions, Docker
---

# Write E2E Tests - Testy End-to-End w Playwright

## Overview

Procedura pisania testów E2E w Playwright z Page Object Model.
Testy uruchamiane w kontenerze Docker, seedowanie danych przez API.

## When to Use

- Nowy feature wymaga pokrycia testami E2E
- Istniejący test wymaga aktualizacji po zmianach w UI
- Nowy flow użytkownika do zautomatyzowania

**When NOT to use:**
- Testy jednostkowe backendu (użyj `write-python-tests`)
- Testy komponentów React (użyj testing-library w `frontend/`)
- Manualna eksploracja strony (użyj Playwright MCP Server)

## Process

### 1. Zidentyfikuj flow do przetestowania

Pytania:
- Jaki flow użytkownika testujemy? (np. logowanie, tworzenie zamówienia)
- Jakie dane są potrzebne? (użytkownicy, produkty, itp.)
- Czy test wymaga autentykacji?
- Jakie asercje są kluczowe?

### 2. Stwórz lub zaktualizuj Page Object

```
e2e/pages/{nazwa}.page.ts
```

Zasady POM:
- Każda strona/komponent ma osobny plik
- Użyj `getByTestId()` jako główny selektor
- Metody opisujące akcje użytkownika (nie techniczne)
- Zwracaj `Promise<void>` lub `Promise<Locator>`

### 3. Stwórz fixture (jeśli potrzebny)

```
e2e/fixtures/{nazwa}.fixture.ts
```

- Seedowanie danych: przez API, NIE przez UI
- Auth state: storageState, logowanie raz
- Cleanup: usuń dane po teście

### 4. Napisz test

```
e2e/tests/{feature}/{nazwa}.spec.ts
```

Struktura testu:
```typescript
import { test, expect } from '@playwright/test';
import { MyPage } from '../../pages/my.page';

test.describe('Feature Name', () => {
  test('should do expected behavior', async ({ page }) => {
    // Arrange - przygotuj dane i nawiguj
    const myPage = new MyPage(page);
    await myPage.goto();

    // Act - wykonaj akcje
    await myPage.doSomething();

    // Assert - sprawdź wynik
    await expect(myPage.successMessage).toBeVisible();
  });
});
```

### 5. Uruchom i zweryfikuj

```bash
# Jeden test
make e2e TEST=tests/auth/login.spec.ts

# Cały suite
make e2e

# Raport
make e2e-report
```

### 6. Sprawdź determinizm

Odpal test 3 razy - musi przejść za kazdym razem:
```bash
make e2e && make e2e && make e2e
```

## Checklist nowego testu

- [ ] Page Object utworzony dla każdej strony w flow
- [ ] Selektory oparte na `data-testid`
- [ ] Dane seedowane przez API fixture
- [ ] Brak `waitForTimeout` - użyj smart waits
- [ ] Test niezależny od innych testów
- [ ] Test przechodzi 3x z rzędu
- [ ] Raport HTML czytelny

## See also

- Rola: `.agents/roles/e2e-engineer.md`
- Workflow: `.agents/workflows/as-e2e-engineer.md`
