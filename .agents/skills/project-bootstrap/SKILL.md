---
name: project-bootstrap
description: Użyj przy rozpoczynaniu nowego projektu na bazie startera - uzupełnia kontekst, dostosowuje role i tworzy początkowy DT
---

# Bootstrap projektu - inicjalizacja projektu

## Przegląd

Jednorazowy skill uruchamiany na początku projektu (po `make setup`).
Wypełnia kontekst projektu, dostosowuje role agentów, tworzy pierwszy DT.

## Kiedy używać

- Nowy projekt oparty na tym starter kicie
- Kiedy użytkownik uruchomi `/project-init`
- Kiedy `.agents/context/project-overview.md` zawiera `[TODO]`

**Kiedy NIE używać:**
- Projekt już zainicjalizowany (brak `[TODO]` w context)
- Zmiana istniejącego projektu

## Proces

### 1. Zbierz informacje od użytkownika

Zapytaj o:
- **Nazwa projektu** (użyj w CLAUDE.md, .env)
- **Opis projektu** (2-3 zdania)
- **Stack technologiczny** (backend, frontend, baza danych)
- **Moduły biznesowe** (co system będzie robił)
- **Architektura** (monolith / microservices / API-only)
- **Cloud provider** (AWS / GCP / inne / brak)

### 2. Wypełnij pliki kontekstowe

Na podstawie odpowiedzi:

| Plik | Co wypełnić |
|------|------------|
| `.agents/context/project-overview.md` | Stack, moduły, diagram |
| `CLAUDE.md` | Zamień `[PROJECT_NAME]` na nazwę |
| `.env` | Stwórz na podstawie potrzeb projektu |

### 3. Dostosuj role agentów

Na podstawie stacku:

| Jeśli... | To... |
|----------|-------|
| Brak frontendu (API-only) | Usuń `frontend-engineer.md` z `.agents/roles/` |
| Django zamiast FastAPI | Dostosuj `backend-engineer.md` |
| Brak UI framework | Dostosuj `frontend-engineer.md` |
| GitHub Actions zamiast GitLab CI | Dostosuj `platform-engineer.md` |

### 4. Stwórz dokumentację architektoniczną

- Wypełnij `docs/dt/ (architektura)`
- Dodaj diagram architektury (mermaid)
- Uzasadnij wybory technologiczne

### 5. Stwórz pierwszy DT

- `DT-001: Project Setup`
- Zakres: infrastruktura (Docker, DB), struktura backendu, ewentualnie frontend
- Użyj szablonu: `docs/szablony/_dt.md`

### 6. Wywołaj Platform Engineera

Zaproponuj:
- Stworzenie `infra/docker-compose.yml`
- Stworzenie Dockerfiles
- Konfiguracja Makefile (jeśli trzeba)

### 7. Zweryfikuj

Sprawdź:
- [ ] Brak `[TODO]` w `project-overview.md`
- [ ] CLAUDE.md ma nazwę projektu
- [ ] Role są dostosowane do stacku
- [ ] DT-001 istnieje
- [ ] `make setup` można uruchomić

## Zobacz też

- Workflow: `.agents/workflows/project-init.md`
- Context: `.agents/context/README.md`
- Skille: `.agents/skills/README.md`
