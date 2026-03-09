---
description: Zamknięcie Dokumentu Technicznego po implementacji - raport + review
---

# Complete DT (zamknięcie po implementacji)

## Kroki

1. **Stwórz raport implementacji**
   - Skopiuj szablon: `docs/szablony/_dt-raport.md`
   - Zapisz jako: `docs/dt/{obszar}/DT-{NNN}_raport.md` (obok oryginalnego DT)

2. **Wypełnij raport**
   - Podsumowanie co zostało zaimplementowane
   - Tabela zmian (plik -> opis zmiany)
   - Zaktualizowana dokumentacja
   - Checklist weryfikacji (kontenery, testy, migracje, logi)
   - Follow-upy (jeśli są)

3. **Zmień status DT na `IN_REVIEW`**
   - W pliku DT zmień `Status: DRAFT` lub `IN_PROGRESS` na `IN_REVIEW`

4. **Poczekaj na review quality-gate**
   - Quality gate sprawdzi raport i kod
   - Jeśli zatwierdzi bez uwag -> przejdź do kroku 5
   - Jeśli ma uwagi -> popraw i wróć do kroku 2

5. **Zmień status DT na `DONE`**
   - Dopiero po zatwierdzeniu review
   - W pliku DT zmień `Status: IN_REVIEW` na `DONE`

> [!IMPORTANT]
> Status `DONE` może być ustawiony **wyłącznie** po zatwierdzeniu review.
> Nigdy nie zamykaj DT bez przejścia przez review.
