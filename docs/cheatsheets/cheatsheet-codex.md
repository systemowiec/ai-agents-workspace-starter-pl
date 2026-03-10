# OpenAI Codex CLI - ściąga 2026

> Źródła: oficjalna dokumentacja OpenAI (developers.openai.com/codex),
> Shipyard cheatsheet, GitHub openai/codex.

---

## Instalacja

```bash
npm i -g @openai/codex
# lub
brew install --cask codex
```

Wymaga: Node.js 18+, macOS lub Linux (Windows przez WSL).
Konto: ChatGPT Plus ($20/mies.) lub API key.

```bash
codex                    # uruchom interaktywny TUI
codex "opisz zadanie"   # od razu z promptem
```

Autentykacja:
```bash
# Opcja A: OAuth przez przeglądarkę (automatycznie przy pierwszym uruchomieniu)
# Opcja B: API key
export OPENAI_API_KEY="sk-..."
codex
```

Brak natywnego `/init`. Codex czyta AGENTS.md automatycznie.

---

## Hierarchia plików kontekstowych

```
~/.codex/AGENTS.md               Globalny - preferencje użytkownika
./AGENTS.md                      Projektowy - reguły projektu
./podkatalog/AGENTS.md           Lokalny - najbliższy do pliku wygrywa
~/.codex/config.toml             Konfiguracja CLI
```

Codex CLI szuka AGENTS.md w kolejności:
1. `~/.codex/AGENTS.md` (preferencje użytkownika)
2. `AGENTS.md` w rootcie projektu (reguły projektowe)
3. `AGENTS.md` w bieżącym katalogu (reguły lokalne)

Codex NIE czyta: CLAUDE.md, GEMINI.md, .cursor/rules/.

---

## Konfiguracja (config.toml)

```toml
# ~/.codex/config.toml
model = "gpt-5.4"
approval_policy = "on-request"

[project]
project_doc_max_bytes = 65536
project_doc_fallback_filenames = ["CLAUDE.md", "README.md"]

[tools]
web_search = false

[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp"]

[sandbox_workspace_write]
network_access = true
```

---

## Tryby bezpieczeństwa (sandbox)

| Tryb | Flaga | Opis |
|---|---|---|
| **Read Only** | `-s read-only` | Tylko odczyt - do audytów |
| **Workspace Write** | `-s workspace-write` | Domyślny - zapis w workspace |
| **Full Access** | `-s danger-full-access` | Pełny dostęp - niebezpieczne |

```bash
codex -s read-only "Przejrzyj kod pod kątem bezpieczeństwa"
codex -s workspace-write "Implementuj nowy endpoint"
codex --full-auto "Napraw failing testy"
```

### Approval policies

| Polityka | Opis |
|---|---|
| `on-request` | Domyślna - pyta przed komendami |
| `never` | Nigdy nie pyta (CI/CD) |
| `--full-auto` | Auto-run + auto-approve |

---

## Slash commands

| Komenda | Działanie |
|---|---|
| `/model` | Zmień model |
| `/plan` | Wejdź w tryb planowania |
| `/clear` | Wyczyść sesję (nowa rozmowa) |
| `/new` | Nowa rozmowa (bez czyszczenia terminalu) |
| `/resume` | Wznów poprzednią sesję |
| `/fork` | Sklonuj rozmowę do nowego wątku |
| `/copy` | Kopiuj ostatni output |
| `/status` | Pokaż model, politykę, tokeny |
| `/permissions` | Zmień politykę approval |
| `/personality` | Zmień styl komunikacji |
| `/mention` | Dodaj plik do kontekstu |
| `/mcp` | Pokaż podłączone MCP serwery |
| `/apps` | Przeglądaj dostępne konektory |
| `/review` | Code review (wbudowany) |
| `/theme` | Zmień motyw kolorystyczny |
| `/experimental` | Toggle eksperymentalnych funkcji |

---

## Tryby uruchomienia

### Interaktywny (TUI)
```bash
codex                              # standardowy
codex --full-auto "zadanie"        # bez potwierdzeń
codex --search "zadanie"           # z live web search
```

### Non-interaktywny (exec)
```bash
codex exec "Zaktualizuj CHANGELOG"
codex exec --full-auto "Napraw testy"
cat logs.txt | codex exec "Wyjaśnij błędy"
```

### Cloud tasks
```bash
codex cloud                        # interaktywny picker
codex cloud exec --env ENV_ID "Zadanie"
```

---

## MCP (Model Context Protocol)

Konfiguracja w `~/.codex/config.toml`:

```toml
[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp", "--api-key", "klucz"]

[mcp_servers.my-db]
command = "/usr/local/bin/my-mcp-server"
env = { DB_URL = "postgres://..." }
timeout_secs = 30
```

W sesji: `/mcp` pokazuje podłączone narzędzia.

---

## Modele

| Model | Kiedy |
|---|---|
| **gpt-5.4** | Rekomendowany domyślny - frontier coding + reasoning |
| **gpt-5.3-codex** | Specjalizowany coding model |
| **gpt-5.3-codex-spark** | Ultra-szybki (research preview, Pro only) |

Zmiana w sesji: `/model`
Zmiana przy starcie: `codex -m gpt-5.4 "zadanie"`

---

## Flagi CLI

### Globalne

| Flaga | Opis |
|---|---|
| `-m, --model` | Wybierz model |
| `-s, --sandbox` | Polityka sandboxa (read-only / workspace-write / danger-full-access) |
| `--full-auto` | Auto-approve wszystko |
| `-a, --approval-policy` | Polityka approval (on-request / never) |
| `--add-dir` | Dodatkowe katalogi z dostępem write |
| `-w, --cwd` | Working directory |
| `--search` | Live web search |
| `--oss` | Użyj lokalnego modelu (np. Ollama) |
| `--profile` | Profil konfiguracji z config.toml |
| `-c key=value` | Override konfiguracji |

### Exec-specific

| Flaga | Opis |
|---|---|
| `--json` | Output w formacie JSONL |
| `--resume` | Wznów poprzednią sesję |

---

## Skróty klawiszowe (TUI)

| Skrót | Działanie |
|---|---|
| `Ctrl+L` | Wyczyść ekran (zachowaj rozmowę) |
| `Ctrl+C` | Przerwij bieżące zadanie |
| `Ctrl+G` | Otwórz edytor ($VISUAL/$EDITOR) do pisania promptu |
| `Ctrl+O` | Wybierz cloud environment |
| `/` | Otwórz menu slash commands |
| `@` | Autouzupełnianie ścieżek plików |

---

## Codzienny workflow

```bash
cd projekt
codex                       # uruchom TUI
/status                     # sprawdź model i ustawienia
"Opisz zadanie"             # podaj prompt
/plan "Opisz feature"       # tryb planowania (opcjonalnie)
Review plan -> approve
/review                     # code review po implementacji
/copy                       # kopiuj output
```

Dla CI/CD:
```bash
codex exec --full-auto "Zaktualizuj CHANGELOG i uruchom testy"
```

---

## Dobre praktyki

- AGENTS.md to jedyny plik kontekstowy - trzymaj go zwięzłym i konkretnym
- Read-only mode (`-s read-only`) do audytów - bezpieczne przeglądanie kodu
- `--full-auto` TYLKO w izolowanych środowiskach (CI/CD, sandbox)
- `/plan` dla złożonych zmian - weryfikuj plan przed implementacją
- Git repo = siatka bezpieczeństwa - zawsze pracuj w repo z commitami
- Batch everything - Codex lepiej działa z parallel tool calls
- `@plik` do dodawania kontekstu - CLI nie widzi automatycznie otwartych plików

---

## Integracja ze starterem

W naszym starterze Codex CLI używa:

| Element | Lokalizacja |
|---|---|
| Punkt wejścia | `AGENTS.md` (root) |
| Konfiguracja | `.codex/config.toml` |
| Skille | Ręcznie - wskaż `@.agents/skills/nazwa/SKILL.md` |
| Role | Ręcznie - wskaż `@.agents/roles/nazwa.md` |
| Workflowy | Ręcznie - wskaż `@.agents/workflows/nazwa.md` |
| MCP | `~/.codex/config.toml` sekcja `[mcp_servers]` |
