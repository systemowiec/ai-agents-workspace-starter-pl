# Współtworzenie projektu

Dziękujemy za chęć rozwijania `ai-agents-workspace-starter-pl`.
To repozytorium jest wersją źródłową w języku polskim.
Równoległe repozytorium angielskie (`ai-agents-workspace-starter`) jest utrzymywane jako tłumaczenie i powinno pozostać merytorycznie zgodne z tym repo.

## Zasady językowe

- Issues i dyskusje: po polsku lub po angielsku, oba języki są akceptowane
- Git (commity, tagi, nazwy branchy, tytuły PR, opisy release): **po angielsku**
- Kod: nazwy zmiennych, funkcji, klas i modułów po angielsku
- Dokumentacja w tym repo: po polsku, z pełną diakrytyką

## Co warto zgłaszać i rozwijać

- Poprawki do `README.md`, `AGENTS.md`, `CLAUDE.md` i dokumentacji
- Usprawnienia ról agentów, workflowów, skilli i reguł IDE
- Dodatkowe szablony dokumentów w `docs/szablony/`
- Konfiguracje dla kolejnych narzędzi AI
- Poprawki jakościowe: spójność terminologii, precyzja instrukcji, zgodność PL/EN

## Zasada synchronizacji PL / EN

- Zmiany merytoryczne w wersji polskiej powinny zostać odzwierciedlone w repozytorium angielskim
- Zmiany w wersji angielskiej nie powinny zmieniać znaczenia względem wersji polskiej
- Nazwy plików, ścieżek i nagłówków w repozytorium angielskim powinny być w języku angielskim
- Jedyny dopuszczalny polski element w repozytorium angielskim to link `Polska wersja` w `README.md`

## Jak wnosić zmiany

1. Sprawdź, czy podobne Issue lub PR już nie istnieją
2. Dla większych zmian najpierw otwórz Issue i opisz problem oraz proponowany kierunek
3. Drobne poprawki dokumentacyjne lub językowe możesz zgłosić bez wcześniejszego Issue
4. Forkuj repozytorium i utwórz branch zgodnie z konwencją
5. Wprowadź zmiany w możliwie najmniejszym sensownym zakresie
6. Jeśli zmiana wpływa na obie wersje językowe, uwzględnij to w opisie PR
7. Otwórz Pull Request z jasnym opisem: co się zmieniło, dlaczego i czy wymagana jest synchronizacja w drugim repo

## Lista kontrolna przed Pull Request

- Zmiana jest zgodna z aktualnym stanem repozytorium i dokumentacji
- Treść jest precyzyjna, techniczna i jednoznaczna
- Nie zmieniasz sensu instrukcji między wersją PL i EN
- Linki, nazwy repozytoriów i ścieżki są zgodne z rzeczywistością
- Commit message jest po angielsku i zgodny z Conventional Commits

## Konwencje

- **Nazewnictwo branchy**: `{type}/opis-zmiany`
- Dozwolone typy branchy: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `infra`, `ci`
- Conventional commits: `feat:`, `fix:`, `refactor:`, `docs:`, `test:`, `infra:`, `ci:`
- Standardowe myślniki: tylko `-` (ASCII 45), nigdy em-dash ani en-dash
- Brak estymacji godzinowych w dokumentach
- Minimal comments - kod ma być self-documenting
- Nie commituj sekretów, kluczy ani danych środowiskowych

## Licencja

Wysyłając wkład do tego repozytorium, zgadzasz się, że będzie on udostępniany na licencji MIT zgodnie z plikiem `LICENSE`.
