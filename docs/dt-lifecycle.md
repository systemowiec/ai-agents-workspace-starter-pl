# Cykl życia Dokumentu Technicznego (DT)

## Statusy

```
DRAFT -> DESIGN -> PLANNED -> IN_PROGRESS -> REVIEW -> DONE
                                    ^            |
                                    |  (FAIL)    |
                                    +------------+
```

| Status | Kto | Co się dzieje |
|---|---|---|
| **DRAFT** | Analyst | Zbieranie wymagań, acceptance criteria, draft DT |
| **DESIGN** | Architect | Plan techniczny, ryzyka, przypisanie ról |
| **PLANNED** | Engineer(s) | Planowanie implementacji per rola |
| **IN_PROGRESS** | Engineer(s) | Implementacja + raporty impl |
| **REVIEW** | Quality Gate | Audyt, scoring, verdykt |
| **DONE** | - | DT zamknięty, wszystkie raporty gotowe |

## Pełny flow

```
FAZA 1: INICJACJA (DRAFT)
  Użytkownik opisuje potrzebę
      |
  Analyst:
    - Pytania doprecyzowujące
    - Acceptance criteria
    - Draft DT wg docs/szablony/_dt.md
    - Status: DRAFT

FAZA 2: PROJEKTOWANIE (DESIGN)
  Architect:
    - Uzupełnia plan techniczny (architektura, pliki, ryzyka)
    - Określa KTÓRE ROLE są wymagane (sekcja "Role" w DT)
    - Tworzy raport: docs/agent-reports/DT-NNN/arch.md
    - Status: DESIGN

FAZA 3: PLANOWANIE (PLANNED)
  Każda przypisana rola planuje swoją część:
    - Backend Engineer: plan backendowy
    - Frontend Engineer: plan frontendowy
    - Platform Engineer: plan infrastrukturalny
    - E2E Engineer: plan testów
  Status: PLANNED

FAZA 4: IMPLEMENTACJA (IN_PROGRESS)
  Każda rola implementuje swoją część i tworzy raport:
    docs/agent-reports/DT-NNN/impl-backend-v1.md
    docs/agent-reports/DT-NNN/impl-frontend-v1.md
    docs/agent-reports/DT-NNN/impl-platform-v1.md
  Status: IN_PROGRESS -> REVIEW

FAZA 5: AUDYT (REVIEW)
  Quality Gate:
    - Audytuje CAŁOŚĆ (wszystkie raporty impl)
    - Tworzy: docs/agent-reports/DT-NNN/review-v1.md
    - Verdykt:
      PASS        -> FAZA 6
      CONDITIONAL -> poprawki (v2), powrót do FAZY 4
      FAIL        -> re-implementacja (v2), powrót do FAZY 4

FAZA 6: ZAMKNIĘCIE (DONE)
  - Status DT: DONE
  - Raport końcowy: docs/agent-reports/DT-NNN/summary.md
  - Workflow: .agents/workflows/complete-dt.md
```

## Które role są wymagane?

Nie każdy DT wymaga wszystkich ról. Architect w fazie DESIGN określa:

| Rola | Zawsze wymagana? | Kiedy potrzebna |
|---|---|---|
| **Analyst** | TAK | Każdy DT zaczyna się od analizy wymagań |
| **Architect** | TAK | Każdy DT wymaga planu technicznego |
| **Backend Engineer** | Zależy | Nowe API, logika biznesowa, migracje |
| **Frontend Engineer** | Zależy | Nowe UI, komponenty, strony |
| **Platform Engineer** | Zależy | Docker, CI/CD, infra |
| **E2E Engineer** | Zależy | Testy E2E nowych flow |
| **Quality Gate** | TAK | Każdy DT przechodzi audyt |

## Struktura raportów

```
docs/agent-reports/
  DT-005/
    arch.md              # analiza architekta
    impl-backend-v1.md   # backend - pierwsza implementacja
    impl-frontend-v1.md  # frontend - pierwsza implementacja
    review-v1.md         # audyt (CONDITIONAL)
    impl-backend-v2.md   # backend - poprawki
    review-v2.md         # re-review (PASS)
    summary.md           # raport końcowy
  DT-006/
    ...
```

### Format nazwy raportu

```
{typ}-{rola}-v{iteracja}.md
```

| Typ | Rola (opcjonalna) | Opis |
|---|---|---|
| `arch` | - | Raport architekta (jeden) |
| `impl` | `backend`, `frontend`, `platform`, `e2e` | Raport implementacji per rola |
| `review` | - | Raport audytu Quality Gate |
| `summary` | - | Raport końcowy (zamknięcie DT) |

## Sekcja "Role" w szablonie DT

Architect wypełnia tę tabelę w fazie DESIGN:

```markdown
## Role

| Rola | Wymagana | Zakres |
|---|---|---|
| analyst | TAK | Wymagania, acceptance criteria |
| architect | TAK | Plan techniczny, ryzyka |
| backend-engineer | TAK/NIE | [opis zakresu] |
| frontend-engineer | TAK/NIE | [opis zakresu] |
| platform-engineer | TAK/NIE | [opis zakresu] |
| e2e-engineer | TAK/NIE | [opis zakresu] |
| quality-gate | TAK | Audyt końcowy |
```
