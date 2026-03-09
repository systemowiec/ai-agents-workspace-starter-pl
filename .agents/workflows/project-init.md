---
description: Inicjalizacja nowego projektu - opis, architektura, pierwsze DT
---

# Project Init

Workflow do uruchomienia nowego projektu opartego na tym starterze.

> **WYMAGANY SKILL**: Przed rozpoczęciem przeczytaj `.agents/skills/project-bootstrap/SKILL.md`

## Kroki

1. **UZUPEŁNIJ nazwę projektu**
   - `CLAUDE.md` (linia 1) - zamień `[PROJECT_NAME]` na nazwę projektu
   - `.env` - ustaw `PROJECT_NAME`

2. **Uruchom Bootstrap Agent** (skill: `project-bootstrap`)
   - Wypełnij `.agents/context/project-overview.md` (stack, moduły, architektura)
   - Wypełnij `.agents/context/api-map.md` (jeśli masz endpointy)
   - Wypełnij `.agents/context/model-relationships.md` (jeśli masz encje)
   - Dostosuj role agentów do stacku:
     - Brak frontendu? Usuń `frontend-engineer.md`
     - Django zamiast FastAPI? Dostosuj `backend-engineer.md`

3. **Zweryfikuj konfigurację agenta**
   - Otwórz `AGENTS.md` - upewnij się, że reguły pasują do projektu
   - Sprawdź czy architektura z sekcji 5 jest poprawna (Hexagonal / Layered)
   - Przeczytaj `.agents/rules/global.md` - 4 Cardinal Rules

4. **Dostosuj konfigurację per IDE**
   - Claude Code: sprawdź `.claude/settings.json` (permissions)
   - Cursor: sprawdź `.cursor/rules/global.mdc` (czy reguły są aktualne)
   - Codex CLI: agent czyta `AGENTS.md` automatycznie

5. **Stwórz dokument architektoniczny**
   - Otwórz `docs/dt/ (architektura)`
   - Wypełnij sekcje: kontekst, stos technologiczny, uzasadnienia
   - Uzupełnij diagram architektury

6. **Stwórz dokumentację projektu**
   - Zaktualizuj `docs/README.md` z opisem projektu
   - Wymień moduły, które będą rozwijane

7. **Stwórz pierwsze Dokumenty Techniczne**
   - Skopiuj szablon z `docs/szablony/_dt.md`
   - Umieść jako `DT-001_project-setup.md`

8. **Skonfiguruj `.env`**
   - Uruchom `make setup` (interaktywny kreator)
   - LUB ręcznie: skopiuj `.env` do `.env` i dostosuj
   - Wygeneruj `SECRET_KEY`: `openssl rand -hex 32`

9. **Zbuduj kontenery**
   ```bash
   make build
   make up
   ```

10. **Zweryfikuj health**
    ```bash
    make status
    ```

11. **Potwierdź że agent czyta konfigurację**
    - W Antigravity: zapytaj "Jakie są reguły Docker First w tym projekcie?"
    - W Cursor: sprawdź czy `.cursor/rules/global.mdc` jest aktywny
    - W Claude Code: zapytaj "Przeczytaj AGENTS.md i potwierdź reguły"
