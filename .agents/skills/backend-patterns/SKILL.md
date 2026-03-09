---
name: backend-patterns
description: >
  Wzorce i szablony dla backend-dev. Czytaj gdy implementujesz nowy kod.
---

# Skill: Backend Patterns

> Wzorce i szablony dla backend-dev. Czytaj gdy implementujesz nowy kod.

---

## FastAPI Router Template

```python
from fastapi import APIRouter, Depends, HTTPException, status
from app.schemas.resource import ResourceCreate, ResourceResponse
from app.services.resource_service import ResourceService
from app.core.dependencies import get_resource_service, get_current_user
from app.domain.entities import User

router = APIRouter(prefix="/api/v1/resources", tags=["resources"])

@router.post("/", response_model=ResourceResponse, status_code=status.HTTP_201_CREATED)
async def create_resource(
    payload: ResourceCreate,
    current_user: User = Depends(get_current_user),
    service: ResourceService = Depends(get_resource_service),
) -> ResourceResponse:
    """Tworzy nowy resource."""
    return await service.create(payload, created_by=current_user)
```

## Repository Pattern

```python
# domain/ports.py - interfejs (port)
from abc import ABC, abstractmethod
from typing import Optional
from app.domain.entities import Resource

class ResourceRepositoryPort(ABC):
    @abstractmethod
    async def get_by_id(self, resource_id: int) -> Optional[Resource]: ...

    @abstractmethod
    async def save(self, resource: Resource) -> Resource: ...

# repositories/sqlalchemy_resource.py - implementacja (adapter)
class SQLAlchemyResourceRepository(ResourceRepositoryPort):
    def __init__(self, session: AsyncSession):
        self._session = session

    async def get_by_id(self, resource_id: int) -> Optional[Resource]:
        result = await self._session.execute(
            select(ResourceModel).where(ResourceModel.id == resource_id)
        )
        row = result.scalar_one_or_none()
        return row.to_domain() if row else None
```

## Service (Use Case) Template

```python
from app.domain.ports import ResourceRepositoryPort
from app.domain.exceptions import ResourceNotFoundError
from app.schemas.resource import ResourceCreate, ResourceResponse

class ResourceService:
    def __init__(self, repo: ResourceRepositoryPort):
        self._repo = repo

    async def create(self, payload: ResourceCreate, created_by) -> ResourceResponse:
        resource = Resource(name=payload.name, created_by=created_by.id)
        saved = await self._repo.save(resource)
        return ResourceResponse.model_validate(saved)

    async def get_by_id(self, resource_id: int) -> ResourceResponse:
        resource = await self._repo.get_by_id(resource_id)
        if not resource:
            raise ResourceNotFoundError(resource_id)
        return ResourceResponse.model_validate(resource)
```

## Huey Task Template

```python
from app.core.huey_config import huey
from app.core.logging import get_logger

logger = get_logger(__name__)

@huey.task(retries=3, retry_delay=60)
def process_heavy_task(resource_id: int) -> dict:
    """Idempotentny task. Safe to retry."""
    logger.info("task_started", resource_id=resource_id)
    try:
        # ... logika
        return {"status": "completed", "resource_id": resource_id}
    except Exception as e:
        logger.error("task_failed", resource_id=resource_id, error=str(e))
        raise
```

## Exception Hierarchy

```python
class AppError(Exception):
    """Base exception."""
    def __init__(self, message: str, code: str = "INTERNAL_ERROR"):
        self.message = message
        self.code = code

class NotFoundError(AppError):
    def __init__(self, entity: str, entity_id: int):
        super().__init__(f"{entity} {entity_id} not found", "NOT_FOUND")

class ValidationError(AppError):
    def __init__(self, message: str, field: str | None = None):
        super().__init__(message, "VALIDATION_ERROR")
        self.field = field

class AuthorizationError(AppError):
    def __init__(self, message: str = "Insufficient permissions"):
        super().__init__(message, "FORBIDDEN")
```

## Pydantic Settings

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    redis_url: str
    secret_key: str
    debug: bool = False
    log_level: str = "INFO"
    cors_origins: list[str] = ["http://localhost:3000"]

    model_config = {"env_file": ".env"}
```

## Dockerfile (multi-stage)

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /build
COPY pyproject.toml requirements/ ./
RUN pip install --no-cache-dir --prefix=/install -r requirements/prod.txt

FROM python:3.12-slim AS production
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app
COPY --from=builder /install /usr/local
COPY app/ ./app/
USER appuser
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
    CMD python -c "import httpx; httpx.get('http://localhost:8000/health')"
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## Zależności baseline

```toml
[project]
requires-python = ">=3.12"
dependencies = [
    "fastapi>=0.115", "uvicorn[standard]>=0.30",
    "pydantic>=2.9", "pydantic-settings>=2.5",
    "sqlalchemy[asyncio]>=2.0", "asyncmy>=0.2",
    "alembic>=1.14", "huey[redis]>=2.5", "redis>=5.0",
    "structlog>=24.0", "httpx>=0.27",
    "python-jose[cryptography]>=3.3", "passlib[bcrypt]>=1.7",
    "tenacity>=9.0",
]
[project.optional-dependencies]
dev = [
    "pytest>=8.0", "pytest-asyncio>=0.24", "pytest-cov>=5.0",
    "httpx>=0.27", "factory-boy>=3.3",
    "ruff>=0.8", "mypy>=1.13", "pre-commit>=4.0",
]
```
