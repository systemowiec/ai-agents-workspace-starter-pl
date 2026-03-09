---
description: Szybki code review - bez pełnego scoringu
---

Szybki review bez pełnego scoringu quality-gate.
Procedura: `.agents/workflows/code-review.md`

1. Przeczytaj `AGENTS.md` - globalne reguły
2. Sprawdź ostatnie zmiany (git diff lub wskazane pliki)
3. Zweryfikuj:
   - Architektura zachowana (domain -> application -> adapters -> interface)
   - Type hints kompletne
   - Brak hardcoded values
   - Testy są
   - Docker First zachowane
   - Conventional Commits
4. Podaj krótkie podsumowanie: APPROVE / APPROVE WITH CHANGES / REQUEST CHANGES
