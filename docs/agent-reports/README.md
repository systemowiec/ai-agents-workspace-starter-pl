# Raporty agentów

Raporty między agentami są przechowywane w plikach i pozostają dostępne po zamknięciu sesji.

## Typy raportów

| Typ | Format | Kto pisze | Kto czyta |
|-----|--------|-----------|-----------|
| Implementacja | `DT-NNN/impl-{rola}-v{N}.md` | backend/frontend/platform/e2e-engineer | quality-gate, architect |
| Review | `DT-NNN/review-v{N}.md` | quality-gate | engineer(s) |
| Architektura | `DT-NNN/arch.md` | architect | engineer(s) |
| Podsumowanie | `DT-NNN/summary.md` | quality-gate | wszyscy |

## Cykl życia

```
Architect: analiza -> arch-*.md -> wiadomość do engineer z wytycznymi
Engineer: implementacja -> make lint && make test -> impl-*.md -> wiadomość do quality-gate
Quality Gate: review -> review-*.md
  PASS/CONDITIONAL -> wiadomość do leada "DT done"
  FAIL -> wiadomość do engineer z listą poprawek -> nowy cykl
```

## Zasady

- Raporty są zapisywane w Git (przetrwają compaction kontekstu)
- Jeden raport na DT i na etap
- NIGDY nie usuwaj raportów - to historia projektu
- Format: Markdown z tabelami i checklistami
