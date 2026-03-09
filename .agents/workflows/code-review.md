---
description: Code review i walkthrough implementacji
---

# Code Review

Procedura review kodu po implementacji - weryfikacja jakości, zgodności z architekturą i standardami.

## 1. Przed review

- Przeczytaj DT/zadanie i acceptance criteria
- Zidentyfikuj zmodyfikowane pliki
- Sprawdź branch i commity

## 2. Checklist jakości

### Architektura
- [ ] Zgodność z architekturą projektu (domain -> application -> adapters -> interface)?
- [ ] Brak circular dependencies?
- [ ] Backward compatibility zachowana?
- [ ] Breaking changes udokumentowane?

### Kod
- [ ] TDD - testy istnieją i pokrywają kluczową logikę?
- [ ] Klasy granularne (<200 linii, Single Responsibility)?
- [ ] Interfejsy/Protokoły dla kluczowych kontraktów?
- [ ] Dependency Injection (brak hardcoded deps)?
- [ ] Brak `any`, `object`, magic numbers?
- [ ] Nazwy semantyczne i w konwencji projektu?
- [ ] Obsługa błędów spójna z resztą projektu?
- [ ] Type hints na każdej funkcji?

### Infrastruktura
- [ ] Migracje prawidłowe (nie ręczne SQL)?
- [ ] Docker-only execution (brak lokalnych komend)?
- [ ] Zmienne env nie hardcoded?

### Dokumentacja
- [ ] Raport implementacji w `docs/agent-reports/`?
- [ ] Conventional Commit messages?

## 3. Weryfikacja działania

```bash
make test
make status
make logs
```

- [ ] Kontenery działają bez błędów?
- [ ] Testy przechodzą?
- [ ] Logi czyste (brak ERROR/WARNING)?

## 4. Walkthrough

Po review stwórz dokument walkthrough:

```markdown
# Walkthrough: [Nazwa zadania]

## Kontekst
- **Ticket:** DT-NNN
- **Branch:** feat/DT-NNN-slug

## Co zostało zaimplementowane
- Zmiana 1
- Zmiana 2

## Pliki zmodyfikowane
| Plik           | Typ zmiany | Opis |
| -------------- | ---------- | ---- |
| `path/file.py` | Nowy       | Opis |

## Decyzje projektowe
- Dlaczego wybrano podejście X over Y

## Jak przetestować
```bash
make test
```

## Potencjalne ulepszenia
- Future improvement 1
```

## 5. Werdykt

| Decyzja                    | Kiedy                                   |
| -------------------------- | --------------------------------------- |
| **APPROVE**                | Kod spełnia standardy, testy przechodzą |
| **APPROVE WITH CHANGES**   | Drobne poprawki, nie blokujące          |
| **REQUEST CHANGES**        | Poważne problemy, wymaga przeróbki      |
