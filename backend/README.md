# Backend

> Ten katalog jest celowo pusty. Jest częścią starter kita.

## Dla agenta

Kiedy użytkownik poprosi o stworzenie backendu:

1. **Przeczytaj** `AGENTS.md` (root) - architektura, reguły, Cardinal Rules
2. **Przeczytaj** `.agents/context/project-overview.md` - stack technologiczny
3. **Wezwij Platform Engineera** (`/as-platform-engineer`) do:
   - Stworzenia `infra/docker-compose.yml` z serwisami backend
   - Stworzenia `infra/backend/Dockerfile` (multi-stage)
   - Dodania targetów do `Makefile`
4. **Dopiero potem** implementuj backend w tym katalogu

## Oczekiwana struktura (Hexagonal Architecture)

```
backend/
├── app/
│   ├── domain/              # Business logic, entities, ports (ABC)
│   ├── application/         # Use cases, service flows (no I/O)
│   ├── infrastructure/      # Adapters (DB, Redis, external APIs)
│   │   ├── adapters/
│   │   └── config/
│   ├── interfaces/          # HTTP layer (routers/views)
│   │   └── api/
│   ├── schemas/             # Pydantic models (request/response)
│   ├── workers/             # Background tasks (Huey/Celery)
│   ├── core/                # Config, security, middleware
│   └── tests/               # Mirror app structure
├── alembic/                 # Database migrations
└── requirements/            # Python dependencies
```

## Skille do użycia

- `.agents/skills/backend-patterns/SKILL.md`
- `.agents/skills/add-endpoint/SKILL.md`
- `.agents/skills/database-patterns/SKILL.md`
- `.agents/skills/database-migration/SKILL.md`
- `.agents/skills/write-python-tests/SKILL.md`
