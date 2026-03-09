---
name: database-migration
description: Use when creating or running database migrations - Alembic, SQLAlchemy, schema changes "Step-by-step guide to create and run database migrations (Alembic + SQLAlchemy)"
---

# Skill: Database Migration

Database migration procedures using Alembic with SQLAlchemy async.

## Creating a New Migration

### 1. Define or Update the Model

Create or update SQLAlchemy model in `backend/app/infrastructure/adapters/db/models.py`.
Ensure the model inherits from `Base` and has proper `__tablename__`.

### 2. Generate Migration

```bash
# Auto-generate from model changes
make db-migrate msg="add users table"

# Equivalent Docker command:
# docker compose -f infra/docker-compose.yml exec backend alembic revision --autogenerate -m "add users table"
```

### 3. Review Migration

Check the generated file in `backend/alembic/versions/`:
- Are all columns correct?
- Are indexes and constraints included?
- Does the migration handle `upgrade()` and `downgrade()` properly?
- Is MariaDB dialect compatible? (no PostgreSQL-specific syntax)

### 4. Apply Migration

```bash
# Apply all pending migrations
make db-upgrade

# Equivalent: alembic upgrade head
```

### 5. Verify

```bash
# Check current migration status
make db-shell
# Then: SHOW TABLES; DESCRIBE my_table;
```

## Rollback

```bash
# Rollback last migration
make db-downgrade

# Equivalent: alembic downgrade -1
```

## Data Migrations

For seed data or data transformations:

```python
# alembic/versions/xxx_seed_default_settings.py
from alembic import op
import sqlalchemy as są

def upgrade():
    op.execute(
        "INSERT INTO settings (key, value) VALUES ('app_name', 'My App')"
    )

def downgrade():
    op.execute("DELETE FROM settings WHERE key = 'app_name'")
```

## Rules

- NEVER run alembic locally - ALWAYS through Docker (`make db-migrate`, `make db-upgrade`)
- ALWAYS review auto-generated migrations before applying
- ALWAYS include `downgrade()` function (rollback support)
- One migration per logical change
- Test rollback works before committing
- Use raw SQL for data migrations, Alembic ops for schema migrations

---

## See also (MANDATORY)

- **Before implementation** - read `workflows/dt-development.md`
- **Model patterns** - read `skills/database-patterns.md`
- **After migration** - run `workflows/post-impl-verify.md`
