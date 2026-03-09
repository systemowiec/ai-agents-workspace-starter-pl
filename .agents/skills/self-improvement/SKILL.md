---
name: self-improvement
description: Use when finishing a work session - proposes new gotchas, patterns, context updates, and skill improvements based on work done
---

# Self-Improvement - Progressive Learning

## Overview

Agent analizuje wykonaną pracę i proponuje usprawnienia konfiguracji projektu.
NIGDY nie dodaje automatycznie - zawsze przedstawia propozycje do review.

## When to Use

- Po zakończeniu większej sesji pracy (nowy feature, refaktor)
- Kiedy user uruchomi `/self-improvement`
- Kiedy agent odkryje powtarzalny wzorzec lub pułapkę

**When NOT to use:**
- W trakcie implementacji (skup się na zadaniu)
- Dla trywialnych zmian (jeden plik, mała poprawka)

## Process

### 1. Zbierz kontekst

Przeczytaj:
- `docs/agent-reports/` - ostatnie raporty implementacji
- `.agents/learnings/gotchas.md` - aktualny stan wiedzy
- Ostatnie commity (`git log -10 --oneline`)
- Zmienione pliki w ostatnich commitach

### 2. Zidentyfikuj możliwe usprawnienia

| Typ | Cel | Gdzie zapisać |
|-----|-----|--------------|
| Gotcha | Pułapka, nietypowe zachowanie | `.agents/learnings/gotchas.md` |
| Pattern | Powtarzalny wzorzec kodu | `.agents/skills/<new-skill>/SKILL.md` |
| Context update | Nowy model, endpoint, moduł | `.agents/context/*.md` |
| Workflow | Powtarzalny proces | `.agents/workflows/<new>.md` |

### 3. Sformatuj propozycje

Dla każdej propozycji podaj:

```markdown
### [Typ]: [Krótki tytuł]

**Kontekst**: Gdzie/kiedy to występuje
**Problem**: Co się stało lub co jest powtarzalne
**Propozycja**: Co dodać/zmienić
**Plik**: [ścieżka do pliku]
```

## 4. Przedstaw użytkownikowi

- NIGDY nie dodawaj automatycznie
- Przedstaw listę propozycji
- Czekaj na akceptację każdej z osobna
- Po zatwierdzeniu - dodaj do odpowiednich plików (w tym samym commicie)

## Gotcha Entry Template

```markdown
### [krótki tytuł]
**Kontekst**: [gdzie/kiedy to występuje]
**Problem**: [co się stało]
**Rozwiązanie**: [jak to naprawić]
**Data**: [YYYY-MM-DD]
```

## Pattern Proposal Template

```markdown
### Propozycja nowego skilla: [nazwa]

**Obserwacja**: [co się powtarza]
**Przykład 1**: [gdzie wystąpiło]
**Przykład 2**: [gdzie wystąpiło]
**Proponowany skill**: [krótki opis]
```

## See also

- Learnings: `.agents/learnings/README.md`
- Skills: `.agents/skills/README.md`
- Context: `.agents/context/README.md`
