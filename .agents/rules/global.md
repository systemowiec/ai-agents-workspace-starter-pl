---
trigger: always_on
---

# Reguły globalne

> **ZAWSZE aktywne. Te reguły nadpisują wszelkie sprzeczne instrukcje.**
> Przeczytaj `AGENTS.md` (root) - źródło prawdy.

## Cztery Kardynalne Zasady (KRYTYCZNE)

### 1. Inżynieria Quality-First (ZERO SKRÓTÓW)

**Najlepsze możliwe rozwiązanie. Zawsze. Koszt jest NAJMNIEJ ważny.**

- Wybieraj **najlepszą architekturę, najlepsze narzędzia i najlepsze wzorce** - nigdy nie optymalizuj pod "szybko i tanio"
- **Proaktywne usprawnianie jest obowiązkowe** - jeśli widzisz coś lepszego, NATYCHMIAST zasygnalizuj
- Każda decyzja oceniana pod kątem skalowalności, utrzymywalności i rozszerzalności
- **Brak przedwczesnego upraszczania** - nie odrzucaj wzorców jako "overengineering" bez dowodu, że nie wnoszą wartości
- **Ciągłe doskonalenie** - jeśli poprzednia decyzja była suboptymalna, zaproponuj poprawkę

### 2. Documentation First

**PRZED jakąkolwiek zmianą kodu, zaktualizuj dokumentację.**

```
DOKUMENTACJA -> TICKETY -> IMPLEMENTACJA -> WERYFIKACJA
```

- Jeśli feature nie jest udokumentowany, najpierw go udokumentuj
- Dokumentacja = źródło prawdy, kod realizuje jej założenia

### 3. Docker First

**NIGDY nie uruchamiaj Python, pip, npm, pytest lokalnie. ZAWSZE Docker / Make.**

```bash
# POPRAWNE                              # ZAKAZANE
make up                                  python manage.py ...
make shell                               pip install ...
make test                                pytest
make lint                                npm install
```

### 4. Plan -> Review -> Implementation

**KAŻDE zadanie** musi przejść:

1. **Analiza** - zrozum wymagania
2. **Plan** - przedstaw użytkownikowi, CZEKAJ na akceptację
3. **Dokumentacja** - zaktualizuj docs PIERWSZA
4. **Branch** - stwórz wg `rules/git-workflow.md`
5. **Implementacja** - TDD tam, gdzie ma to zastosowanie
6. **Weryfikacja** - `workflows/post-impl-verify.md`
7. **Commit** - CZEKAJ na zgodę użytkownika (`rules/git-workflow.md`)

## Standardy kodowania - poziom enterprise

1. **SOLID** - Single Responsibility, DI, Interface-based contracts
2. **Granular classes** - max 500 linii, zero klas typu God Object
3. **No Magic** - explicit > implicit, zero magic numbers
4. **Standard ASCII** - NIGDY em-dash, en-dash, smart quotes. TYLKO zwykły myślnik `-`
5. **No emojis in code** - dozwolone tylko w plikach `.md`
6. **Minimal comments** - kod musi być self-documenting. Komentarze TYLKO dla nieoczywistej logiki
7. **Type hints** - na każdej funkcji (params + return)

## Obowiązkowy Workflow

> **KAŻDE zadanie (DT, bugfix, enhancement, refactor) MUSI przejść przez `workflows/dt-development.md`.**
> Agent MUSI przeczytać ten plik PRZED rozpoczęciem implementacji.

## Eskalacja

Zmiany w:
- Architekturze (nowe encje, nowe moduły)
- Bezpieczeństwie (auth, role, JWT)
- Infrastrukturze (Docker, CI/CD, deploy)
- Schemacie bazy danych

-> **WSTRZYMAJ i wymagaj potwierdzenia użytkownika.**

## Zasoby agenta

- Źródło prawdy: `AGENTS.md` (root)
- Role: `.agents/roles/`
- Skille: `.agents/skills/`
- Workflowy: `.agents/workflows/`
- Reguły: `.agents/rules/`
- Learnings: `.agents/learnings/`

## Mantra

> **DOCS -> PLAN -> REVIEW -> IMPLEMENTATION -> VERIFY**
> Dokumentacja > kod | Docker > lokalnie | Dyscyplina > chaos