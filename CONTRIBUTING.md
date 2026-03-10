# Jak współpracować przy projekcie

## Język

- Issues i dyskusje: po polsku lub po angielsku, oba języki są akceptowane
- Git (commity, tagi, tytuły PR, opisy release): **po angielsku**
- Kod: nazwy zmiennych i funkcji po angielsku, komentarze minimalne

## Co można rozwijać

- Dodatkowe szablony dokumentacji w `docs/szablony/`
- Rozszerzone definicje ról agentów w `.agents/roles/`
- Nowe skille w `.agents/skills/`
- Konfiguracje dla innych narzędzi AI
- Przykładowe projekty używające tego startera
- Poprawki do istniejących instrukcji i README

## Proces

1. Sprawdź czy nie ma już otwartego Issue na ten temat
2. Otwórz Issue z opisem propozycji przed napisaniem kodu
3. Forkuj repozytorium, stwórz gałąź wg konwencji: `feat/opis-zmiany` lub `fix/opis-zmiany`
4. Wprowadź zmiany, commit z opisem po angielsku (conventional commits)
5. Otwórz Pull Request z opisem co i dlaczego zostało zmienione

## Konwencje

- **Branch naming**: `{type}/opis-zmiany` - typy: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`
- Conventional commits: `feat(scope):`, `fix(scope):`, `docs(scope):`
- Standardowe myślniki: tylko `-` (ASCII 45), nigdy em-dash/en-dash
- Brak estymacji godzinowych w dokumentach
- Minimal comments - kod musi być self-documenting
