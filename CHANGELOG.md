# Changelog

Wszystkie istotne zmiany w projekcie są dokumentowane w tym pliku.
Format oparty na [Keep a Changelog](https://keepachangelog.com/pl/1.0.0/).

## [0.1.0] - 2026-03-08

### Dodano

- Hierarchia kontekstu: globalny AGENTS.md + README.md per katalog
- 4 narzędzia CLI/IDE: Claude Code, Antigravity, Cursor, Codex CLI
- 7 ról agentów z zakresem odpowiedzialności i ograniczeniami (Analyst, Architect, Backend Engineer, Frontend Engineer, Platform Engineer, E2E Engineer, Quality Gate)
- 13 skilli w formacie folder-based Anthropic SKILL.md
- 7 workflowów pokrywających cały cykl życia zadania
- Cykl życia Dokumentu Technicznego z 6 statusami (DRAFT -> DESIGN -> PLANNED -> IN_PROGRESS -> REVIEW -> DONE)
- Quality Gate ze scoringiem w 6 kategoriach i mechanizmem VETO
- Szablony dokumentacji: DT, raport implementacji, raport audytu
- Cross-IDE parity: te same role i skille we wszystkich środowiskach
- 4 cheatsheets narzędzi (Claude Code, Antigravity, Cursor, Codex CLI)
- Konwencje językowe z listą słów kluczowych (docs/konwencje-jezykowe.md)
- Dokumentacja cyklu życia DT (docs/dt-lifecycle.md)
- Skill `project-bootstrap` do inicjalizacji nowego projektu
- Skill `brainstorming` z anti-rationalization tables
- Skill `self-improvement` do progressive learning agenta
- Docker Compose jako domyślne środowisko (Docker-First rule)
- Konfiguracja Claude Code: Agent Teams, subagenci, komendy, permissions
- Konfiguracja Cursor: glob-based .mdc rules z aktywacją kontekstową
- Konfiguracja Codex CLI: config.toml z fallback filenames
