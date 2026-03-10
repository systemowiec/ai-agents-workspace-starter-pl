---
name: database-migration
description: Użyj przy tworzeniu lub uruchamianiu migracji bazy danych - Alembic, SQLAlchemy, zmiany schematu. "Przewodnik krok po kroku tworzenia i uruchamiania migracji bazy danych (Alembic + SQLAlchemy)"
---

# Skill: Migracja bazy danych

Procedury migracji bazy danych z użyciem Alembic oraz asynchronicznego SQLAlchemy.

## Tworzenie nowej migracji

### 1. Zdefiniuj lub zaktualizuj model

Utwórz lub zaktualizuj model SQLAlchemy w `backend/app/infrastructure/adapters/db/models.py`.
Upewnij się, że model dziedziczy po `Base` i ma poprawne `__tablename__`.

### 1b. Sprawdź konwencję nazewnictwa migracji

Przed wygenerowaniem migracji agent MUSI sprawdzić istniejące pliki w `backend/alembic/versions/`.

Zasady:
- Jeśli projekt używa numeracji sekwencyjnej, np. `0001_...`, `0002_...`, agent MUSI kontynuować tę samą konwencję
- Jeśli projekt używa innego ustalonego schematu nazw, agent MUSI go zachować 1:1
- Agent NIE może wymyślać własnego formatu nazw plików migracji
- Agent NIE może tworzyć losowych, ogólnych ani nieczytelnych nazw typu `update`, `fix`, `changes`, `test`, `misc`
- Opis migracji ma być krótki, po angielsku, techniczny i jednoznaczny
- Jeśli w repozytorium nie ma jeszcze żadnej migracji ani jawnie opisanej konwencji, agent ma najpierw zapytać użytkownika o oczekiwany format

### 2. Wygeneruj migrację

```bash
# Wygeneruj automatycznie na podstawie zmian modelu
make db-migrate msg="add users table"

# Równoważna komenda Docker:
# docker compose -f infra/docker-compose.yml exec backend alembic revision --autogenerate -m "add users table"
```

### 3. Zweryfikuj migrację

Sprawdź wygenerowany plik w `backend/alembic/versions/`:
- Czy wszystkie kolumny są poprawne?
- Czy indeksy i constraints zostały uwzględnione?
- Czy migracja poprawnie obsługuje `upgrade()` i `downgrade()`?
- Czy jest zgodna z dialektem MariaDB? (bez składni specyficznej dla PostgreSQL)

### 4. Zastosuj migrację

```bash
# Zastosuj wszystkie oczekujące migracje
make db-upgrade

# Odpowiednik: alembic upgrade head
```

### 5. Zweryfikuj

```bash
# Sprawdź aktualny stan migracji
make db-shell
# Następnie: SHOW TABLES; DESCRIBE my_table;
```

## Rollback

```bash
# Wycofaj ostatnią migrację
make db-downgrade

# Odpowiednik: alembic downgrade -1
```

## Migracje danych

Dla seed data albo transformacji danych:

```python
# alembic/versions/xxx_seed_default_settings.py
from alembic import op
import sqlalchemy as sa

def upgrade():
    op.execute(
        "INSERT INTO settings (key, value) VALUES ('app_name', 'My App')"
    )

def downgrade():
    op.execute("DELETE FROM settings WHERE key = 'app_name'")
```

## Zasady

- NIGDY nie uruchamiaj alembica lokalnie - ZAWSZE przez Docker (`make db-migrate`, `make db-upgrade`)
- ZAWSZE sprawdź istniejące nazewnictwo plików w `backend/alembic/versions/` przed utworzeniem nowej migracji
- ZAWSZE weryfikuj auto-generowane migracje przed zastosowaniem
- ZAWSZE uwzględniaj funkcję `downgrade()` (obsługa rollbacku)
- Jedna migracja na jedną logiczną zmianę
- Nazwa migracji musi być zgodna z konwencją projektu, a nie z domysłem agenta
- Przed commitem sprawdź, czy rollback działa
- Dla migracji danych używaj raw SQL, a dla migracji schematu operacji Alembica

---

## Zobacz też (OBOWIĄZKOWE)

- **Przed implementacją** - przeczytaj `workflows/dt-development.md`
- **Wzorce modeli** - przeczytaj `skills/database-patterns.md`
- **Po migracji** - uruchom `workflows/post-impl-verify.md`
