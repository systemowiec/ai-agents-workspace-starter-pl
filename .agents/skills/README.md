# Skille - konwencja i format

## Format skilli (Anthropic SKILL.md standard)

Każdy skill to **folder** z plikiem `SKILL.md` oraz opcjonalnymi zasobami.

### Struktura

```
skills/
  skill-name/
    SKILL.md              # Główna instrukcja (wymagany)
    scripts/              # Skrypty pomocnicze (opcjonalny)
    examples/             # Przykładowy kod (opcjonalny)
    templates/            # Szablony (opcjonalny)
    resources/            # Dodatkowe pliki (opcjonalny)
```

### Format pliku SKILL.md

```markdown
---
name: skill-name
description: Use when [triggering conditions and symptoms]
---

# Nazwa skilla

## Przegląd
Co to jest? 1-2 zdania.

## Kiedy używać
Kiedy używać, kiedy NIE używać.

## Proces / wzorzec
Główna instrukcja.

## Typowe błędy
Co może pójść nie tak.

## Zobacz też
Linki do powiązanych zasobów.
```

### Zasady YAML frontmatter

- **name**: lowercase, hyphens, bez znaków specjalnych
- **description**: zaczyna się od "Use when..." - opisuje KIEDY używać (nie CO robi)
- Max 1024 znaków łącznie

### CSO (Claude Search Optimization)

Description w `SKILL.md` musi być zoptymalizowany pod discovery:
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

## Szablon

Użyj `_template/SKILL.md` jako punkt wyjścia.
