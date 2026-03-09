# Claude Code - Cheatsheet 2026

> Źródła: oficjalna dokumentacja Anthropic, Claude Code Docs.

---

## Instalacja

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Wymaga: Node.js 18+. Konto: Claude Pro ($20/mies.) lub Claude Max ($100-200/mies.).

```bash
cd twoj-projekt
claude
/init          # skanuje codebase, generuje CLAUDE.md
```

---

## Hierarchia plików kontekstowych

```
~/.claude/CLAUDE.md              Globalny - wszystkie projekty
~/CLAUDE.md                      Nadrzędny - monorepo root
./CLAUDE.md                      Projektowy - współdzielony przez Git
./frontend/CLAUDE.md             Podkatalog - ładowany on-demand (*)
```

(*) Subdirectory CLAUDE.md ładowane gdy Claude czyta pliki w tym katalogu.
W praktyce bywa zawodne - bezpieczniej używać @importów w rootowym CLAUDE.md.

### @importy (niezawodne)

W CLAUDE.md możesz importować inne pliki:

```markdown
@AGENTS.md
@.agents/rules/global.md
@.agents/context/project-overview.md
```

Importowane pliki ładują się przy starcie sesji. Rekurencja do 5 poziomów.

### Co przeżywa compaction

- CLAUDE.md - TAK (ponownie czytany z dysku po /compact)
- Instrukcje z rozmowy - NIE (mogą zginąć)
- Pliki w .claude/rules/ - TAK

---

## Architektura 4-warstwowa

| Warstwa | Opis | Lokalizacja |
|---|---|---|
| **L1 - CLAUDE.md** | Trwały kontekst i reguły | `./CLAUDE.md` |
| **L2 - Skills** | Automatycznie wywoływane paczki wiedzy | `.claude/skills/<n>/SKILL.md` |
| **L3 - Hooks** | Bramki bezpieczeństwa i automatyzacja | `.claude/settings.json` |
| **L4 - Agents** | Subagenci z własnym kontekstem | `.claude/agents/<n>.md` |

---

## Struktura katalogów

```
projekt/
├── CLAUDE.md                    # Pamięć projektu (ładowana przy starcie)
├── .claude/
│   ├── settings.json            # Uprawnienia, hooks, env
│   ├── settings.local.json      # Lokalne nadpisania (gitignored)
│   ├── skills/                  # Skills (folder-based, SKILL.md)
│   │   ├── code-review/SKILL.md
│   │   └── testing/SKILL.md
│   ├── commands/                # Slash commands (/nazwa)
│   │   └── deploy.md
│   ├── agents/                  # Subagenci
│   │   └── quality-gate.md
│   └── rules/                   # Reguły (z frontmatter paths:)
│       └── backend-only.md
```

---

## Skills

Pole `description` jest kluczowe - decyduje o auto-aktywacji.

```yaml
---
name: testing-patterns
description: Use when writing or updating tests - Jest, pytest, AAA pattern
allowed-tools: Read, Grep, Glob
---
```

| Scope | Lokalizacja |
|---|---|
| Projektowy | `.claude/skills/<nazwa>/SKILL.md` |
| Personalny | `~/.claude/skills/<nazwa>/SKILL.md` |

Skills ładowane on-demand (progressive disclosure) - tylko nazwa i opis przy starcie, pełna treść dopiero gdy potrzebne.

---

## Subagenci

```yaml
---
name: quality-gate
description: Enterprise Code Auditor. Scoring w 6 kategoriach, VETO < 90%.
tools: Read, Grep, Glob
model: inherit
---
```

Subagenci mogą mieć własną pamięć (pole `memory: user`).

---

## Hooks

Deterministyczne callbacki: PreToolUse, PostToolUse, Notification, SessionStart, TeammateIdle, TaskCompleted.

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "scripts/sec.sh",
        "timeout": 5
      }]
    }]
  }
}
```

Exit codes: `0` = pozwól, `2` = zablokuj.

---

## Uprawnienia (settings.json)

```json
{
  "permissions": {
    "allow": ["Read:*", "Bash:git:*", "Write:*:*.md"],
    "deny": ["Read:env:*", "Bash:sudo:*"]
  }
}
```

---

## Agent Teams (eksperymentalne)

Wymagane: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` w settings.json.

Każdy teammate działa w osobnej instancji, z własnym kontekstem.
Tryb Delegate (Shift+Tab): lead traci dostęp do Edit/Write/Bash, może tylko spawnować i koordynować.

---

## Slash commands

| Komenda | Działanie |
|---|---|
| `/init` | Generuj/aktualizuj CLAUDE.md |
| `/compact` | Kompresuj kontekst (z opcjonalnym focusem) |
| `/clear` | Wyczyść sesję |
| `/memory` | Zarządzaj auto-pamięcią |
| `/context` | Pokaż co zajmuje kontekst |
| `/resume` | Wznów poprzednią sesję |
| `/model` | Zmień model |
| `/copy` | Kopiuj ostatni output |
| `/batch <instrukcja>` | Równoległe zmiany w wielu plikach |
| `/simplify` | Review ostatnich zmian (3 agentów równolegle) |

---

## Skróty klawiszowe

| Skrót | Działanie |
|---|---|
| `Shift + Tab` | Zmień tryb (Plan / Auto Accept / Delegate) |
| `Tab` | Toggle extended thinking |
| `Esc Esc` | Rewind (cofnij rozmowę i/lub kod) |
| `Ctrl+C` | Przerwij bieżące zadanie |
| `Ctrl+T` | Toggle task list |
| `Ctrl+F` | Zabij background agentów |
| `#` | Dodaj notatkę do CLAUDE.md |

---

## Codzienny workflow

```
cd projekt && claude
Shift+Tab+Tab  ->  Plan Mode
Opisz feature
Shift+Tab      ->  Auto Accept (po zatwierdzeniu planu)
/compact       ->  gdy kontekst rośnie
Esc Esc        ->  cofnij jeśli coś poszło nie tak
Commit często
Nowa sesja per feature
```

---

## Best practices

- CLAUDE.md poniżej 150 linii - dłuższe pliki zmniejszają adherencję
- Szczegóły przenoś do osobnych plików z @importami
- Gotchas których Claude nie wywnioskuje - dodaj ręcznie
- Commituj CLAUDE.md do Git - dziel z zespołem
- `/init` to punkt startowy, nie gotowy produkt - dopracuj ręcznie
- Po compaction instrukcje z rozmowy mogą zginąć - ważne reguły trzymaj w CLAUDE.md
- Każda linia w CLAUDE.md powinna przejść test: "czy usunięcie spowoduje błąd?"

---

## Integracja ze starterem

W naszym starterze Claude Code używa:

| Element | Lokalizacja |
|---|---|
| Entry point | `CLAUDE.md` (root) |
| Subagenci | `.claude/agents/*.md` (thin wrappery do `.agents/roles/`) |
| Slash commands | `.claude/commands/` (implement, audit, review) |
| Skills | `.claude/skills -> ../.agents/skills` (symlink) |
| Uprawnienia | `.claude/settings.json` (Docker-first deny list) |
