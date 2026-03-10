---
description: Kompletny workflow implementacji DT - od analizy do commita
---

# Workflow implementacji DT

> **Główny workflow.** KAŻDE zadanie MUSI przejść przez ten proces.
> Przeczytaj także: `rules/git-workflow.md` (branch, commit, push).

---

## Dwa tryby

### Tryb A: Istniejący DT (DT-NNN)

Użytkownik przekazuje DT (np. "zrób DT-001" albo wkleja link do task trackera).
Agent:
1. Czyta treść DT
2. Tworzy branch (`feat/DT-001-slug`)
3. Przechodzi przez fazy 1-8

### Tryb B: Zadanie ad-hoc od użytkownika

Użytkownik opisuje zadanie słownie (np. "dodaj encję User").
Agent:
1. NIE ma DT - zaczyna od analizy i planu
2. Branch jest tworzony dopiero PO akceptacji planu
3. Na końcu (faza 7) proponuje utworzenie DT-NNN

> W obu trybach obowiązuje ten sam proces opisany poniżej.

---

## Faza 1: Analiza

1. Przeczytaj wymagania (task tracker, `docs/dt/` albo opis od użytkownika)
2. Zidentyfikuj moduły, serwisy i pliki do zmiany
3. Sprawdź istniejące testy i powiązany kod
4. Przejrzyj dokumentację (`docs/`)

---

## Faza 2: Plan

Przygotuj plan implementacji zawierający:

- **Pliki do zmiany** - istniejące i nowe
- **Architektura** - czy zmiana jest zgodna z architekturą projektu? Których warstw dotyczy?
- **Zależności** - inne DT, serwisy, migracje
- **Ryzyka** - breaking changes, backward compatibility
- **Strategia testów** - które testy trzeba dopisać lub zaktualizować

**Przedstaw plan użytkownikowi i CZEKAJ na akceptację.**

> NIE zaczynaj kodowania przed akceptacją planu.

---

## Faza 3: Dokumentacja (PRZED kodem!)

> **Documentation First** - `rules/global.md`, Kardynalna Zasada nr 2

- Zaktualizuj dokumentację projektu zgodnie z planowaną zmianą
- Jeśli dodajesz encję -> zaktualizuj dokumentację modelu danych
- Jeśli dodajesz endpoint -> zaktualizuj dokumentację API
- Jeśli zmieniasz auth -> zaktualizuj dokumentację auth

---

## Faza 4: Branch

> Jeśli jeszcze tego nie zrobiłeś, przeczytaj `rules/git-workflow.md`.

- **Tryb A** (DT istnieje): branch powinien już istnieć od fazy 1
- **Tryb B** (ad-hoc): utwórz branch teraz, po akceptacji planu

```bash
git checkout -b feat/DT-NNN-slug
```

---

## Faza 5: Implementacja

Pracuj zgodnie z kolejnością architektoniczną projektu (np. domain -> application -> adapters -> interface).

Zasady:
- Najpierw Entity/Model, potem Repository/Service, na końcu Router/View
- Dla UI używaj komponentów frameworka UI (najpierw sprawdź `frontend/src/components/`)
- Dark/Light mode jest obowiązkowy przy zmianach frontendowych
- Tylko Docker - zero komend lokalnych

---

## Faza 6: Weryfikacja

**OBOWIĄZKOWE - uruchom `workflows/post-impl-verify.md` PRZED zgłoszeniem zakończenia pracy.**

1. Przebuduj zmieniony serwis
2. Sprawdź zdrowie kontenerów
3. Sprawdź logi (bez ERROR/Traceback)
4. Uruchom testy
5. Sprawdź TypeScript (jeśli frontend był zmieniany)

**Jeśli COKOLWIEK się nie powiedzie - napraw problem i powtórz od kroku 1.**

---

## Faza 7: Commit

> Przeczytaj `rules/git-workflow.md` - pełne zasady commitowania.

**NIGDY nie commituj automatycznie. CZEKAJ na zgodę użytkownika.**

Po uzyskaniu zgody zaproponuj:
```bash
git add <files>
git commit -m "{type}({scope}): short description"
```

---

## Faza 8: Raport

Utwórz raport implementacji w `docs/agent-reports/DT-NNN/impl-backend-v1.md`:

```markdown
# Implementacja: [DT/zadanie]

## Podsumowanie
Co zostało zaimplementowane.

## Zmiany
- `path/to/file.py` - opis
- `path/to/other.py` - opis

## Zaktualizowana dokumentacja
- Zaktualizowano dokumentację API

## Weryfikacja
- [ ] Kontenery działają
- [ ] Testy przechodzą
- [ ] Logi są czyste

## Follow-upy
- Usprawnienie 1 w przyszłości
```

---

## Faza 9: Podsumowanie

Przedstaw użytkownikowi:
1. Co zostało zaimplementowane
2. Jakie zmiany w dokumentacji zostały wykonane
3. Komendę commita (oczekującą na akceptację)
4. Lokalizację raportu
5. Ewentualne follow-upy

> **NIGDY nie mów "zrobione" bez fazy 6 (weryfikacji).**
> **NIGDY nie pushuj do gita - użytkownik kontroluje push.**
