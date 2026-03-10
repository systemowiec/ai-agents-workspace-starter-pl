# Dokumentacja projektu

## Struktura

```
docs/
├── agent-reports/      # Raporty agentów (impl, review, arch, audit)
├── cheatsheets/        # Cheat sheety narzędzi (Claude Code, Antigravity, Cursor, Codex)
├── dt/                 # Dokumenty Techniczne
│   └── README.md        # Konwencje i auto-numeracja
├── szablony/           # Szablony dokumentów
│   ├── _dt.md          # Szablon DT
│   ├── _dt-raport.md   # Szablon raportu implementacji
│   └── _audit-raport.md # Szablon raportu audytu QG
└── README.md           # Ten plik
```

## Dokumenty Techniczne (DT)

- Szablon DT: `szablony/_dt.md`
- Szablon raportu: `szablony/_dt-raport.md`
- Counter: auto-numeracja przez skanowanie istniejących plików
- Tworzenie: workflow `/create-dt`
- Zamykanie: workflow `/complete-dt`

## Raporty agentów

Wszystkie raporty w `agent-reports/`:
- `DT-NNN/impl-{rola}-v{N}.md` - raporty implementacji
- `DT-NNN/review-v{N}.md` - raporty review (quality-gate)
- `DT-NNN/arch.md` - raporty architektoniczne
- `DT-NNN/summary.md` - raporty końcowe

Protokół: `agent-reports/README.md`

## Cheat sheety

Materiały referencyjne w `cheatsheets/`:
- `cheatsheet-claude-code.md` - Claude Code CLI
- `cheatsheet-antigravity.md` - Google Antigravity
- `cheatsheet-cursor.md` - Cursor IDE
- `cheatsheet-codex.md` - OpenAI Codex CLI

## Konwencje

- Słowa kluczowe i poprawność językowa: `konwencje-językowe.md`
- Język dokumentacji: **polski**
- Język kodu: **angielski**
- Język Git (commity, tagi, branchy, PR): **angielski**
- Standardowe myślniki: tylko `-` (ASCII 45), nigdy em-dash/en-dash
- Brak estymacji godzinowych w jakimkolwiek dokumencie
