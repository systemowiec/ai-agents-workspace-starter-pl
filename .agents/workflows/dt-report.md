---
description: Tworzenie DT oraz raportów implementacyjnych
---

# Workflow raportu DT

> Procedura tworzenia DT oraz przygotowywania raportów implementacyjnych.

---

## Kiedy używać

- Po zaimplementowaniu zadania (faza 8 w `dt-development.md`)
- Gdy proponujesz nowy DT dla pracy ad-hoc

---

## 1. Format DT (Markdown)

```markdown
# DT-NNN: Tytuł

## Cel
Krótki opis celu.

## Zakres
- [ ] Zadanie 1
- [ ] Zadanie 2

## Acceptance Criteria
- Kryterium 1
- Kryterium 2

## Zależności
- DT-YYY (jeśli dotyczy)

## Notatki techniczne
Szczegóły implementacyjne.
```

## 2. Kolejny numer DT

Przeskanuj istniejące DT, aby ustalić kolejny numer:

```bash
ls docs/dt/*/DT-*.md 2>/dev/null | sort -t- -k2 -n | tail -1
```

## 3. Raport implementacji

Po zakończeniu pracy nad DT utwórz raport w `docs/agent-reports/`:

```markdown
# DT-NNN: Tytuł - Raport implementacji

## Podsumowanie
Co zostało zaimplementowane.

## Zmiany
- `backend/app/domain/entities.py` - nowa encja
- `backend/app/application/service.py` - logika biznesowa
- `backend/app/tests/test_service.py` - testy jednostkowe

## Zaktualizowana dokumentacja
- Zaktualizowano dokumentację API
- `docs/dt/` - zaktualizowano diagram

## Weryfikacja
- [ ] Kontenery działają
- [ ] Testy przechodzą
- [ ] Logi są czyste

## Follow-upy
- DT-YYY: dodanie powiązanego feature'a
```

---

## Zasady

- **NIGDY** nie wpisuj estymacji czasowych ani wzmianek o AI w DT
- **NIGDY** nie skracaj ani nie upraszczaj raportów
- Raporty w `docs/agent-reports/DT-NNN/` stosują nazewnictwo: `impl-{rola}-v{N}.md`
