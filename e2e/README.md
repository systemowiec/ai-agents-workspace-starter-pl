# Testy E2E

Katalog testów end-to-end (Playwright + TypeScript).

## Struktura

```
e2e/
├── tests/           # Testy per feature
├── pages/           # Page Object Model
├── fixtures/        # Auth state, seedowanie danych
├── helpers/         # API client, utilities
└── playwright.config.ts
```

## Komendy

```bash
make e2e            # Uruchom wszystkie testy
make e2e-report     # Pokaz ostatni raport HTML
```

## Dokumentacja

- Rola: `.agents/roles/e2e-engineer.md`
- Skill: `.agents/skills/write-e2e-tests/SKILL.md`
- Workflow: `.agents/workflows/as-e2e-engineer.md`
