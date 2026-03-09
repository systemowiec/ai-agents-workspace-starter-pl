---
name: brainstorming
description: Use when starting any creative work - creating features, designing architecture, planning refactors. Explores requirements and design before implementation.
---

# Brainstorming - Design Before Code

## Overview

Pomóż przekształcić pomysły w pełni dopracowane koncepcje przez naturalny dialog.
Zrozum kontekst projektu, zadawaj pytania, zaproponuj rozwiązania, uzyskaj aprobatę PRZED kodowaniem.

> **HARD GATE**: NIE KODUJ przed zatwierdzeniem designu. Dotyczy KAŻDEGO projektu bez względu na postrzeganą prostotę.

## When to Use

- Nowy feature do zaimplementowania
- Refaktor wymagający decyzji architektonicznych
- Zmiana API contracts
- Nowy moduł lub serwis
- Zmiana infrastruktury

**When NOT to use:**
- Bugfix z jasną przyczyną
- Zmiana konfiguracji
- Aktualizacja dokumentacji

## Red Flags - jeśli tak myślisz, to STOP

| Myśl | Rzeczywistość |
|------|---------------|
| "To za proste na design" | Proste rzeczy = niezbadane założenia. Design może być krótki. |
| "Już wiem co zrobić" | Wiedza != plan. Napisz design. |
| "Szybko to naprawię" | Szybkie fixy bez designu = regression |
| "User chce żeby zrobić, nie planować" | Instrukcja mówi CO, nie JAK. Design mówi JAK. |

## Process

### 1. Zbadaj kontekst projektu
- Sprawdź pliki, docs, ostatnie commity
- Przeczytaj `.agents/context/project-overview.md`
- Zidentyfikuj powiązane moduły

### 2. Zadawaj pytania - PO JEDNYM na raz
- Nie zasypuj wieloma pytaniami
- Preferuj pytania wielokrotnego wyboru
- Skup się na: cel, ograniczenia, kryteria sukcesu

### 3. Zaproponuj 2-3 podejścia
- Przedstaw opcje z trade-offs
- Prowadź z rekomendowaną opcją i uzasadnieniem
- Uwzględnij YAGNI - usuń zbędne features

### 4. Zaprezentuj design
- Skaluj każdą sekcję do złożoności
- Po każdej sekcji pytaj czy wygląda dobrze
- Pokryj: architekturę, komponenty, data flow, error handling, testy

### 5. Zapisz design
- Zapisz do `docs/plans/YYYY-MM-DD-<topic>-design.md`
- Commituj dokument designu

### 6. Przejdź do implementacji
- Stwórz plan implementacji (workflow `dt-development.md`)
- NIE koduj bez planu

## Key Principles

- **Jedno pytanie na raz** - nie przytłaczaj
- **YAGNI bezlitosnie** - usuń zbędne features z każdego designu
- **Eksploruj alternatywy** - zawsze 2-3 podejścia
- **Inkrementalna walidacja** - prezentuj, uzyskaj aprobatę, idź dalej

## See also

- Workflow: `.agents/workflows/dt-development.md`
- Workflow: `.agents/workflows/create-dt.md`
