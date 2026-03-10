# Cursor IDE - ściąga 2026

> Źródła: oficjalna dokumentacja Cursor, cursor.com/changelog, community guides.

---

## Instalacja

Pobierz z [cursor.com](https://cursor.com). Bazuje na VS Code - rozszerzenia i ustawienia przenoszą się automatycznie.

Konto: Free (ograniczone), Pro ($20/mies.), Business ($40/mies./os.), Enterprise.

Brak natywnego `/init`. Alternatywy:
- `/Generate Cursor Rules` - wpisz w chat, Cursor wygeneruje reguły dla projektu
- `cursor-kit init -t cursor` - narzędzie community (npm)

---

## Hierarchia plików kontekstowych

```
AGENTS.md                        Otwarty standard - ładowany automatycznie
.cursor/rules/*.mdc              Reguły Cursor z YAML frontmatter
~/.cursor/rules/*.mdc            Globalne reguły użytkownika
```

Cursor NIE czyta:
- CLAUDE.md (specyficzny dla Claude Code)
- README.md z podkatalogów (automatycznie)
- GEMINI.md (specyficzny dla Antigravity)

---

## 4 warstwy reguł

| Warstwa | Lokalizacja | Zakres |
|---|---|---|
| **User Rules** | Cursor Settings > Rules | Globalne, wszystkie projekty |
| **Team Rules** | Dashboard zespołu | Współdzielone w zespole |
| **Project Rules** | `.cursor/rules/*.mdc` | Wersjonowane w repo |
| **Agent Rules** | `AGENTS.md` w rootcie | Otwarty standard, cross-tool |

Priorytet: User > Team > Project > Agent Rules.

---

## Format pliku .mdc

```yaml
---
description: "Reguły dla kodu backendowego Python"
globs: "backend/**/*.py"
alwaysApply: false
---

# Markdown content z instrukcjami
```

### 4 tryby aktywacji

| Tryb | Frontmatter | Kiedy aktywne |
|---|---|---|
| **Always** | `alwaysApply: true` | Zawsze, w każdej rozmowie |
| **Auto Attached** | `globs: "*.py"` | Gdy edytujesz pasujące pliki |
| **Agent Requested** | `description: "..."` (globs puste) | Agent sam decyduje na podstawie opisu |
| **Manual** | Wszystko puste | Tylko gdy użyjesz `@nazwaReguły` |

---

## Struktura katalogów

```
projekt/
├── AGENTS.md                    # Agent Rules (cross-tool)
├── .cursor/
│   ├── rules/                   # Project Rules (.mdc)
│   │   ├── global.mdc           # alwaysApply: true
│   │   ├── role-backend.mdc     # globs: backend/**
│   │   ├── role-frontend.mdc    # globs: frontend/**
│   │   └── docs.mdc             # globs: docs/**
│   └── mcp.json                 # Konfiguracja MCP serwerów
```

---

## Kontekst w promptach (@-mentions)

| Symbol | Działanie |
|---|---|
| `@plik.ts` | Dołącz plik do kontekstu |
| `@folder/` | Dołącz folder |
| `@Web` | Przeszukaj internet |
| `@Docs` | Przeszukaj dokumentację |
| `@Git` | Kontekst z historii Git |
| `@Codebase` | Przeszukaj cały codebase |
| `@nazwaReguły` | Załaduj ręcznie regułę .mdc |
| `@Notepad` | Dołącz zawartość Notepada |

---

## Tryby pracy

| Tryb | Opis | Skrót |
|---|---|---|
| **Agent Mode** | Autonomiczny - planuje, edytuje, wykonuje komendy | Domyślny w Composer |
| **Normal Mode** | Asystent - file-aware, bez autonomii | Cmd+L (Chat) |
| **Plan Mode** | Tworzy plan przed implementacją | Shift+Tab w Composer |
| **YOLO Mode** | Auto-run komend bez potwierdzeń | Ustawienia |

---

## Skróty klawiszowe (macOS / Windows-Linux)

### AI

| Skrót | Działanie |
|---|---|
| `Cmd+I` / `Ctrl+I` | Otwórz Composer (Agent Mode) |
| `Cmd+L` / `Ctrl+L` | Otwórz Chat |
| `Cmd+K` / `Ctrl+K` | Inline edit / pytanie w terminalu |
| `Cmd+.` / `Ctrl+.` | Toggle Agent Mode |
| `Tab` | Zaakceptuj sugestię AI |
| `Cmd+Shift+K` / `Ctrl+Shift+K` | Zastosuj zmiany z chata |

### Nawigacja

| Skrót | Działanie |
|---|---|
| `Cmd+P` / `Ctrl+P` | Quick Open (pliki) |
| `Cmd+Shift+P` / `Ctrl+Shift+P` | Command Palette |
| `Cmd+Shift+F` / `Ctrl+Shift+F` | Szukaj w projekcie |
| `Cmd+B` / `Ctrl+B` | Toggle sidebar |

### Edycja

| Skrót | Działanie |
|---|---|
| `Cmd+Shift+L` / `Ctrl+Shift+L` | Dodaj zaznaczenie do chata |
| `Cmd+D` / `Ctrl+D` | Multi-cursor (zaznacz następne wystąpienie) |
| `Alt+Up/Down` | Przenieś linię |
| `Cmd+/` / `Ctrl+/` | Toggle komentarz |

---

## MCP (Model Context Protocol)

Konfiguracja w `~/.cursor/mcp.json` (globalnie) lub `.cursor/mcp.json` (per projekt):

```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp",
      "headers": { "CONTEXT7_API_KEY": "klucz" }
    }
  }
}
```

---

## Automations (2026)

Always-on agenci uruchamiani triggerami (Slack, Linear, GitHub, PagerDuty, webhooks).
Konfiguracja: cursor.com/automations.
Agent działa w cloud sandbox z dostępem do MCP i modeli.

---

## Cloud Agents (2026)

Agenci chmurowi - każdy we własnej izolowanej VM.
Generują merge-ready PR z artefaktami (video, screenshoty, logi).
Dostępne przez: web, desktop, mobile, Slack, GitHub.

---

## Codzienny workflow

```
Otwórz projekt w Cursor
Cmd+I           ->  Composer (Agent Mode)
Opisz zadanie
Shift+Tab       ->  Plan Mode (opcjonalnie, dla złożonych zmian)
Review diff     ->  Zaakceptuj lub odrzuć zmiany
@plik.ts        ->  Dodaj kontekst gdy potrzeba
Cmd+K           ->  Inline edit na konkretnym fragmencie
```

---

## Dobre praktyki

- Reguły .mdc poniżej 500 linii, im krócej tym lepiej
- Używaj globów do aktywacji kontekstowej (nie ładuj wszystkiego zawsze)
- AGENTS.md dla reguł cross-tool, .cursor/rules/ dla Cursor-specific
- Notepad (@Notepad) do tymczasowego kontekstu między sesjami
- `/Generate Cursor Rules` jako punkt startowy - dopracuj ręcznie
- W `description` pisz KIEDY aktywować, nie CO robi reguła

---

## Integracja ze starterem

W naszym starterze Cursor używa:

| Element | Lokalizacja |
|---|---|
| Punkt wejścia | `.cursor/rules/global.mdc` (alwaysApply) + `AGENTS.md` |
| Role (glob-based) | `.cursor/rules/role-backend.mdc`, `role-frontend.mdc` itd. |
| Docs rules | `.cursor/rules/docs.mdc` (globs: docs/**) |
| Skille | Ręcznie przez `@.agents/skills/nazwa/SKILL.md` |
| MCP | `.cursor/mcp.json` |
