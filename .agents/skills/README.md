# Skills - Konwencja i format

## Format skilli (Anthropic SKILL.md standard)

Każdy skill to **folder** z plikiem `SKILL.md` oraz opcjonalnymi zasobami.

### Struktura

```
skills/
  skill-name/
    SKILL.md              # Główna instrukcja (wymagany)
    scripts/              # Helper scripts (opcjonalny)
    examples/             # Przykładowy kod (opcjonalny)
    templates/            # Szablony (opcjonalny)
    resources/            # Dodatkowe pliki (opcjonalny)
```

### Format SKILL.md

```markdown
---
name: skill-name
description: Use when [triggering conditions and symptoms]
---

# Skill Name

## Overview
Co to jest? 1-2 zdania.

## When to Use
Kiedy używać, kiedy NIE używać.

## Process / Pattern
Główna instrukcja.

## Common Mistakes
Co może pójść nie tak.

## See also
Linki do powiązanych zasobów.
```

### Zasady YAML frontmatter

- **name**: lowercase, hyphens, bez znaków specjalnych
- **description**: zaczyna się od "Use when..." - opisuje KIEDY używać (nie CO robi)
- Max 1024 znaków łącznie

### CSO (Claude Search Optimization)

Description w SKILL.md musi być zoptymalizowany pod discovery:
- Zaczynaj od "Use when..."
- Opisuj SYMPTOMY i SYTUACJE (nie proces)
- Uwzględniaj synonimy i warianty

## Jak dodać nowy skill

1. Sprawdź czy taki skill już nie istnieje
2. Skopiuj `_template/SKILL.md` do nowego folderu
3. Wypełnij frontmatter (name, description)
4. Napisz instrukcje
5. Zaproponuj dodanie użytkownikowi (NIGDY automatycznie)
6. Zaktualizuj `AGENTS.md` sekcja 13 z nowym skillem

## Template

Użyj `_template/SKILL.md` jako punkt wyjścia.
