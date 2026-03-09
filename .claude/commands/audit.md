---
description: Pełny code audit z quality-gate scoring
---

Uruchom quality-gate subagenta.

1. Przeczytaj `.agents/roles/quality-gate.md` - scoring system i checklista
2. Przeczytaj `AGENTS.md` - globalne reguły
3. Przeczytaj `.agents/learnings/gotchas.md` - znane pułapki
4. Zapytaj użytkownika o zakres: last commit / feature X / full audit
5. Wykonaj scoring wg 6 kategorii (architektura, bezpieczeństwo, testy, jakość, kontrakty, docs)
6. Zapisz raport w `docs/agent-reports/review-{YYYYMMDD-HHmm}.md`
7. Jeśli znalazłeś gotcha - zaproponuj wpis do `.agents/learnings/gotchas.md`

WAŻNE: Uruchamiaj ZAWSZE jako osobny subagent (readonly).
NIGDY nie audytuj kodu który sam napisałeś w tej samej sesji.
