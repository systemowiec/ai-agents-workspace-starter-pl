---
name: add-endpoint
description: Użyj przy dodawaniu nowego endpointu REST API - router, serwis, repozytorium, schema, testy. "Przewodnik krok po kroku dodawania nowego endpointu REST API (FastAPI + SQLAlchemy + Pydantic)"
---

# Skill: Dodawanie endpointu REST

Pełna procedura dodawania nowego endpointu REST. Hexagonal Architecture (domain -> application -> adapters -> interface).

## 1. Encja domenowa (`backend/app/domain/entities.py`)

```python
from dataclasses import dataclass
from datetime import datetime

@dataclass
class MyEntity:
    id: int | None = None
    name: str = ""
    is_active: bool = True
    created_at: datetime | None = None
```

## 2. Port domenowy (`backend/app/domain/ports.py`)

```python
from abc import ABC, abstractmethod
from app.domain.entities import MyEntity

class MyEntityRepository(ABC):
    @abstractmethod
    async def get_by_id(self, entity_id: int) -> MyEntity | None: ...

    @abstractmethod
    async def save(self, entity: MyEntity) -> MyEntity: ...

    @abstractmethod
    async def list_active(self) -> list[MyEntity]: ...
```

## 3. Serwis aplikacyjny (`backend/app/application/my_entity_service.py`)

```python
from app.domain.ports import MyEntityRepository
from app.domain.exceptions import NotFoundError
from app.schemas.my_entity import MyEntityCreate, MyEntityResponse

class MyEntityService:
    def __init__(self, repo: MyEntityRepository):
        self._repo = repo

    async def create(self, payload: MyEntityCreate) -> MyEntityResponse:
        entity = MyEntity(name=payload.name)
        saved = await self._repo.save(entity)
        return MyEntityResponse.model_validate(saved)

    async def get_by_id(self, entity_id: int) -> MyEntityResponse:
        entity = await self._repo.get_by_id(entity_id)
        if not entity:
            raise NotFoundError("MyEntity", entity_id)
        return MyEntityResponse.model_validate(entity)
```

## 4. Schemy Pydantic (`backend/app/schemas/my_entity.py`)

```python
from pydantic import BaseModel
from datetime import datetime

class MyEntityCreate(BaseModel):
    name: str

class MyEntityResponse(BaseModel):
    id: int
    name: str
    is_active: bool
    created_at: datetime

    model_config = {"from_attributes": True}
```

## 5. Adapter SQLAlchemy (`backend/app/infrastructure/adapters/db/`)

```python
# models - model ORM
class MyEntityModel(Base):
    __tablename__ = "my_entities"
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(255), nullable=False)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, server_default="CURRENT_TIMESTAMP")

# repository - adapter implementujący port
class SQLAlchemyMyEntityRepository(MyEntityRepository):
    def __init__(self, session: AsyncSession):
        self._session = session

    async def get_by_id(self, entity_id: int) -> MyEntity | None:
        result = await self._session.get(MyEntityModel, entity_id)
        return result.to_entity() if result else None
```

## 6. Router FastAPI (`backend/app/interfaces/api/my_entities.py`)

```python
from fastapi import APIRouter, Depends, status
from app.schemas.my_entity import MyEntityCreate, MyEntityResponse
from app.core.dependencies import get_my_entity_service, get_current_user

router = APIRouter(prefix="/api/v1/my-entities", tags=["my-entities"])

@router.post("/", response_model=MyEntityResponse, status_code=status.HTTP_201_CREATED)
async def create_entity(
    payload: MyEntityCreate,
    current_user=Depends(get_current_user),
    service=Depends(get_my_entity_service),
) -> MyEntityResponse:
    return await service.create(payload)
```

## 7. Migracja Alembic

```bash
make db-migrate msg="add my_entities table"
make db-upgrade
```

## 8. Testy

```bash
make test
```

---

## Zobacz też (OBOWIĄZKOWE)

- **Przed implementacją** - przeczytaj `workflows/dt-development.md`
- **Migracje** - jeśli dodajesz nowy model, przeczytaj `skills/database-migration.md`
- **Testy** - przeczytaj `skills/write-python-tests.md`
- **Po implementacji** - uruchom `workflows/post-impl-verify.md`
