# Context - Kontekst projektu

Ten katalog zawiera pliki kontekstowe, które agent czyta aby zrozumieć projekt.

## Pliki

| Plik | Opis | Kto aktualizuje |
|------|------|----------------|
| `project-overview.md` | Opis projektu, stack, moduły, architektura | Bootstrap Agent / Architect |
| `api-map.md` | Mapa endpointów API | Engineer po dodaniu endpointu |
| `model-relationships.md` | Relacje między encjami (ER diagram) | Engineer po dodaniu modelu |

## Kiedy aktualizować

- Po dodaniu nowego modułu -> `project-overview.md`
- Po dodaniu/zmianie endpointu -> `api-map.md`
- Po dodaniu/zmianie modelu -> `model-relationships.md`
- Po refaktorze architektury -> wszystkie

## Format

- Markdown z tabelami i diagramami mermaid
- Placeholdery `[TODO]` dla niezainicjalizowanych sekcji
- Agent NIE dodaje wpisów automatycznie - proponuje i czeka na akceptację
