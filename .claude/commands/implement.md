---
description: Implementacja ticketa z pełnym cyklem raportowania
---

## Kroki

1. Przeczytaj `.agents/roles/backend-engineer.md` - Twoja rola
2. Przeczytaj `.agents/workflows/dt-development.md` - OBOWIĄZKOWY flow
3. Przeczytaj odpowiednie skille z `.agents/skills/`
4. Przeczytaj `.agents/learnings/gotchas.md` - znane pułapki
5. Przeczytaj `AGENTS.md` - globalne reguły

## Workflow

1. Przeanalizuj zadanie
2. Zaproponuj PLAN (pliki do zmiany, moduły, ryzyka)
3. Czekaj na akceptację planu
4. Zaktualizuj dokumentację PRZED kodem (Documentation First)
5. Implementuj
6. Uruchom weryfikację: `.agents/workflows/post-impl-verify.md`
7. Uruchom: `make lint && make test`
8. Zapisz raport w `docs/agent-reports/DT-NNN/impl-backend-v1.md`
9. Poinformuj użytkownika: "Implementacja zakończona. Raport gotowy do review."
