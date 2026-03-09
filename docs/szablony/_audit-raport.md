# Szablon: Raport audytu

## Metadata

- **Data**: [YYYY-MM-DD]
- **Audytor**: quality-gate
- **Zakres**: [backend / frontend / infrastruktura]
- **Wersja kodu**: [commit SHA]

## Scoring

| Kategoria | Waga | Ocena (0-100%) | Ważony wynik |
|-----------|------|---------------|-------------|
| Architektura (SOLID, Hexagonal) | 25% | [X]% | [X * 0.25]% |
| Bezpieczeństwo (auth, secrets, input) | 20% | [X]% | [X * 0.20]% |
| Testy (pokrycie, jakość, izolacja) | 20% | [X]% | [X * 0.20]% |
| Jakość kodu (DRY, KISS, typing) | 15% | [X]% | [X * 0.15]% |
| Kontrakty API (spójność, walidacja) | 10% | [X]% | [X * 0.10]% |
| Dokumentacja (DT, docstrings) | 10% | [X]% | [X * 0.10]% |
| **RAZEM** | 100% | - | **[SUMA]%** |

## Wynik końcowy

| Ocena | Próg | Decyzja |
|-------|------|---------|
| >= 95% | PASS | Można scalić |
| 90-94% | CONDITIONAL PASS | Poprawki wymagane (P2+P3) |
| < 90% | FAIL | Blokada, wymagana re-implementacja |

**Wynik**: [PASS / CONDITIONAL / FAIL] - [SUMA]%

## Findings

### P1 - Critical (blokujące)

| # | Plik | Linia | Problem | Sugestia |
|---|------|-------|---------|----------|
| 1 | [plik] | [linia] | [opis] | [sugestia] |

### P2 - High (wymagane przed merge)

| # | Plik | Linia | Problem | Sugestia |
|---|------|-------|---------|----------|
| 1 | [plik] | [linia] | [opis] | [sugestia] |

### P3 - Low (nice to have)

| # | Plik | Linia | Problem | Sugestia |
|---|------|-------|---------|----------|
| 1 | [plik] | [linia] | [opis] | [sugestia] |

## Routing

| Odbiorca | Zakres |
|----------|--------|
| backend-engineer | P1 + P2 z backendu |
| frontend-engineer | P1 + P2 z frontendu |
| platform-engineer | P1 + P2 z infra |

## Rekomendacje

[Podsumowanie + następne kroki]
