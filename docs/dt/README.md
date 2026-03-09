# Dokumenty Techniczne (DT)

W tym katalogu przechowywane są Dokumenty Techniczne.

## Konwencja nazewnictwa

```
DT-NNN_{krotki-tytul}.md
```

Przykłady:
- `DT-001_user-entity.md`
- `DT-002_playwright-e2e.md`

## Auto-numeracja

Numer nadawany jest automatycznie na podstawie istniejących plików.
Agent skanuje katalog, bierze najwyższy numer i dodaje +1.

## Tworzenie nowego DT

```
/create-dt
```

## Statusy

`DRAFT` -> `DESIGN` -> `PLANNED` -> `IN_PROGRESS` -> `REVIEW` -> `DONE`

Pełny opis cyklu życia: `docs/dt-lifecycle.md`
