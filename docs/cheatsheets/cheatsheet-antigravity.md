# Google Antigravity - ściąga 2026

> Źródła: Google Codelabs, Google Developers Blog, oficjalna dokumentacja Antigravity,
> community guides (aifire.co, antigravity.codes).

---

## Instalacja

Pobierz z [antigravity.google/download](https://antigravity.google/download).
Dostępne na: macOS, Windows, Linux.
Konto: osobiste konto Gmail (public preview, bezpłatne).

Przy pierwszym uruchomieniu wybierz tryb pracy:

| Tryb | Opis |
|---|---|
| **Agent-Driven** | Autopilot - AI pisze kod, tworzy pliki, wykonuje komendy |
| **Agent-Assisted** | Rekomendowany - balans autonomii z kontrolą |
| **Review-Driven** | AI prosi o pozwolenie przed każdą akcją |

Brak natywnego `/init` - konfigurację tworzysz ręcznie lub przez community tools.

---

## Dwa interfejsy

### Widok edytora
Znane środowisko typu VS Code - pliki, terminal i panel czatu agenta (Cmd+L).

### Manager Surface (Mission Control)
Dashboard do uruchamiania, monitorowania i orkiestracji wielu agentów równolegle.
Każde zadanie = osobna instancja agenta z artefaktami (plany, diffy, screenshoty).

---

## Hierarchia plików kontekstowych

```
~/.gemini/GEMINI.md                   Globalny - wszystkie projekty
./AGENTS.md                           Projektowy - natywnie od v1.20.3 (*)
./GEMINI.md                           Projektowy - opcjonalny (legacy)
.agents/rules/*.md                    Reguły workspace (always-on)
.agents/skills/<n>/SKILL.md           Skills (on-demand)
.agents/workflows/<n>.md              Workflows (slash commands)
.gemini/antigravity/brain/            Pamięć agenta (auto-generowana)
```

(*) Od wersji 1.20.3 (marzec 2026) Antigravity ładuje `AGENTS.md` natywnie jako główny
plik kontekstowy projektu. `GEMINI.md` nadal działa, ale nie jest wymagany.

---

## System customizacji (3 warstwy)

### Rules - pasywne, always-on

System instructions - agent uwzględnia je ZAWSZE.

| Zakres | Lokalizacja |
|---|---|
| Globalny | `~/.gemini/GEMINI.md` lub Antigravity Settings > Rules > + Global |
| Workspace | `AGENTS.md` (natywnie od v1.20.3), `.agents/rules/*.md`, lub Settings > Rules > + Workspace |

Przykład:
```markdown
# Code Style
- Framework: FastAPI + React
- Language: TypeScript strict
- Styling: Tailwind CSS

# Forbidden
- Nie uruchamiaj komend lokalnie - tylko Docker / Make
- Nie używaj npm ani pip bezpośrednio
```

### Workflows - zapisane prompty, on-demand

Aktywowane przez `/nazwa` w chacie agenta. Analogia: zapisany prompt wielokrotnego użytku.

| Zakres | Lokalizacja |
|---|---|
| Globalny | `~/.gemini/antigravity/workflows/` |
| Workspace | `.agents/workflows/*.md` |

Format:
```yaml
---
description: Uruchom Quality Gate - audyt kodu
---

# Kroki workflow
1. Przeczytaj raporty implementacji
2. Sprawdź architekturę
3. Wygeneruj scoring
```

### Skills - progressive disclosure

Ładowane dynamicznie, tylko gdy agent uzna że pasują do zadania.
Agent widzi nazwę i opis na starcie - pełna treść ładowana on-demand.

| Zakres | Lokalizacja |
|---|---|
| Globalny | `~/.gemini/antigravity/skills/<n>/SKILL.md` |
| Workspace | `.agents/skills/<n>/SKILL.md` |

Format identyczny jak Anthropic Agent Skills:
```yaml
---
name: database-migration
description: >
  Use when creating or running database migrations.
  Alembic, schema changes, rollback procedures.
---
```

---

## Brain directory (pamięć agenta)

Antigravity automatycznie tworzy `.gemini/antigravity/brain/` w projekcie.
Agent zapisuje tam odkrycia (preferencje, decyzje architektoniczne).
Przyszli agenci czytają ten kontekst przed rozpoczęciem pracy.

---

## MCP (Model Context Protocol)

Konfiguracja: menu "..." > MCP Servers > Manage > View raw config.
Plik: `~/.gemini/antigravity/mcp_config.json`.

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

Po zapisaniu: Manage MCP Servers > Refresh.

Antigravity ma też wbudowany MCP Store z gotowymi integracjami (Firebase, Figma, Google Cloud).

---

## Tryby agenta

| Tryb | Kiedy |
|---|---|
| **Plan Mode** | Złożone zadania - agent tworzy Plan Artifact przed implementacją |
| **Fast Mode** | Szybkie zadania - agent implementuje natychmiast |

---

## Artefakty (Trust Layer)

Agent generuje artefakty - namacalne rezultaty do weryfikacji:
- Listy zadań
- Plany implementacji
- Screenshoty
- Nagrania przeglądarki (browser recordings)

Możesz zostawiać komentarze na artefaktach (jak w Google Docs) - agent je uwzględnia.

---

## Bezpieczeństwo agenta

### Zasady wykonywania komend w terminalu

| Polityka | Opis |
|---|---|
| Auto | Standardowe komendy bez pytania, niebezpieczne wymagają zgody |
| Agent Decides | Agent sam ocenia ryzyko |
| Always Ask | Zawsze pytaj przed wykonaniem |

### Listy Allow/Deny

Konfiguracja: Antigravity Settings > Advanced Settings > Terminal.

```
Allow: make, docker compose, git status, git diff
Deny: rm -rf, git push --force, pip install, npm install
```

---

## Skróty klawiszowe (macOS)

| Skrót | Działanie |
|---|---|
| `Cmd+L` | Przełącz panel agenta (Editor) |
| `Cmd+I` | Inline completion / komenda |
| `Cmd+,` | Ustawienia |
| `Cmd+Shift+P` | Command Palette |
| `Tab` | Zaakceptuj sugestię |

---

## Modele

| Model | Kiedy |
|---|---|
| **Gemini 3 Pro** | Domyślny, najlepszy do autonomicznego kodowania |
| **Claude Sonnet 4.5** | Alternatywa, dobry w planowaniu |
| **GPT-4** | Dostępny jako opcja |

Zmiana modelu: w ustawieniach agenta lub per zadanie w Manager.

---

## Codzienny workflow

```
Otwórz projekt w Antigravity
Manager Surface     ->  dla dużych zadań (multi-agent)
Editor + Cmd+L      ->  dla szybkich zmian (single agent)
Plan Mode           ->  dla złożonych feature'ów
Fast Mode           ->  dla fixów i drobnych zmian
/nazwa-workflow     ->  aktywuj zapisany workflow
Review artefakty    ->  zweryfikuj plan przed implementacją
Komentuj artefakty  ->  feedback bez restartowania agenta
```

---

## Dobre praktyki

- Oddzielny folder per projekt - agent dostaje czysty, focused kontekst
- Biblioteka skilli - zbuduj raz, importuj do każdego projektu
- Plan Mode dla złożonych zmian - weryfikuj plan PRZED implementacją
- Rules krótkie i konkretne - agent lepiej je realizuje
- Artefakty jako trust layer - nie czytaj każdej linii kodu, weryfikuj plan
- Jeden task per agent - nie mieszaj niezwiązanych zadań w jednym prompcie

---

## Integracja ze starterem

W naszym starterze Antigravity używa:

| Element | Lokalizacja |
|---|---|
| Punkt wejścia | `AGENTS.md` (natywnie od v1.20.3) |
| Rules | `.agents/rules/*.md` (always-on) |
| Skille | `.agents/skills/<n>/SKILL.md` (on-demand) |
| Workflows | `.agents/workflows/*.md` (slash commands) |
| Role | `.agents/roles/*.md` (referencja z workflows) |
| MCP | `~/.gemini/antigravity/mcp_config.json` |
