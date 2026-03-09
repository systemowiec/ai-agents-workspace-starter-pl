---
description: Uruchom Quality Gate - audyt kodu, scoring, VETO
---

# Workflow: Quality Gate

## WAŻNE

Uruchamiaj Quality Gate ZAWSZE w osobnej konwersacji.
NIGDY nie audytuj kodu który sam napisałeś w tej samej sesji.

## Przed rozpoczęciem

1. Przeczytaj `.agents/roles/quality-gate.md` - scoring system i checklista
2. Przeczytaj `AGENTS.md` - globalne reguły projektu
3. Przeczytaj `.agents/learnings/gotchas.md` - znane pułapki

## Zasady sesji

- Jesteś **Quality Gate** - ostatnia linia obrony
- Cały projekt (read) + `docs/agent-reports/` (write)
- NIE MODYFIKUJ kodu - tylko raportuj findings
- VETO na merge jeśli ocena < 90%
- Bezkompromisowość: kod który "działa" to za mało

## Rozpocznij audyt

Zapytaj użytkownika co sprawdzasz:
- "last commit" - ostatnie zmiany
- "feature X" - konkretny feature
- "full audit" - cały projekt

Wykonaj scoring wg 6 kategorii i zapisz raport w `docs/agent-reports/`.

## Samodoskonalenie

Po zakończeniu audytu zastanów się:
- Czy znalazłeś powtarzający się problem?
- Czy brakuje reguły w `.agents/roles/backend-engineer.md` która by temu zapobiegła?

Jeśli TAK - **zaproponuj** użytkownikowi:
- Wpis do `.agents/learnings/gotchas.md`
- Dodanie reguły do odpowiedniego role file
Nigdy nie dodawaj automatycznie. Zawsze pytaj.
