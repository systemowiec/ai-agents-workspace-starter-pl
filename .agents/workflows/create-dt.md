---
description: Tworzenie nowego Dokumentu Technicznego (DT) z auto-numeracją
---

# Create DT (Dokument Techniczny)

## Kroki

1. **Znajdź najwyższy numer DT**
   // turbo
   ```bash
   ls docs/dt/*/DT-*.md 2>/dev/null | sort -t- -k2 -n | tail -1
   ```
   Jeśli brak plików - zacznij od DT-001.

2. **Oblicz nowy numer**
   - Weź najwyższy istniejący numer, dodaj 1
   - Formatuj jako 3-cyfrowy: `001`, `002`, ..., `999`

3. **Wybierz obszar**
   Dostępne katalogi w `docs/dt/`:
   - `modules/` - DT dotyczące modułów biznesowych (np. `modules/cart/`, `modules/payments/`)
   - `infrastruktura/` - DT dotyczące Docker, CI/CD, DevOps
   - Utwórz nowy katalog jeśli żaden nie pasuje

4. **Skopiuj szablon**
   // turbo
   ```bash
   cp docs/szablony/_dt.md docs/dt/{obszar}/DT-{NNN}_{krotki-tytul}.md
   ```

5. **Wypełnij dokument**
   - Nadaj tytuł
   - Ustaw status: `DRAFT`
   - Wypełnij: cel, zakres, kryteria akceptacji, notatki techniczne
   - **NIE dodawaj estymacji godzinowej**

6. **Poinformuj użytkownika** o stworzonym DT i jego numerze
