---
name: database-patterns
description: >
  Wzorce dostępu do bazy danych. Czytaj gdy pracujesz z modelami, repozytoriami, migracjami.
---

# Skill: Wzorce bazodanowe

> Wzorce dostępu do bazy danych. Czytaj gdy pracujesz z modelami, repozytoriami, migracjami.

---

## Stack

- **MariaDB 11+** (MySQL-compatible)
- **SQLAlchemy 2.x** async (asyncmy driver)
- **Alembic** dla migracji
- **Kontener**: `infra/docker-compose.yml` -> service `mariadb`

## Connection string

```python
# .env -> DATABASE_URL
DATABASE_URL=mysql+asyncmy://${DB_USER}:${DB_PASSWORD}@mariadb:3306/${DB_NAME}
```

Wewnątrz Dockera używaj hostname `mariadb` (nazwa serwisu), NIE `localhost`.

## Repository Pattern (Hexagonal)

Baza danych jest adapterem. Nigdy nie importuj SQLAlchemy w warstwie domain/application.

```
domain/
  ports.py          -> ABC interfejsy (UserRepository, OrderRepository)
  entities.py       -> Czyste klasy Pythona (dataclass, bez ORM)

application/
  services.py       -> Używa portów (DI), nie wie o SQLAlchemy

infrastructure/
  adapters/
    db/
      models.py     -> SQLAlchemy ORM models (Base, User, Order)
      repositories.py -> Implementacja portów (SQLAlchemy queries)
      session.py    -> AsyncSession factory
```

### Port (interfejs)

```python
from abc import ABC, abstractmethod
from app.domain.entities import User

class UserRepository(ABC):
    @abstractmethod
    async def get_by_id(self, user_id: int) -> User | None: ...

    @abstractmethod
    async def save(self, user: User) -> User: ...

    @abstractmethod
    async def list_active(self) -> list[User]: ...
```

### Adapter (implementacja)

```python
from sqlalchemy import select
from sqlalchemy.ext.asyncio import AsyncSession
from app.domain.ports import UserRepository
from app.domain.entities import User
from app.infrastructure.adapters.db.models import UserModel

class SQLAlchemyUserRepository(UserRepository):
    def __init__(self, session: AsyncSession):
        self._session = session

    async def get_by_id(self, user_id: int) -> User | None:
        result = await self._session.get(UserModel, user_id)
        return result.to_entity() if result else None

    async def save(self, user: User) -> User:
        model = UserModel.from_entity(user)
        self._session.add(model)
        await self._session.flush()
        return model.to_entity()
```

### Model ORM

```python
from sqlalchemy import Column, Integer, String, Boolean, DateTime
from sqlalchemy.orm import DeclarativeBase
from app.domain.entities import User

class Base(DeclarativeBase):
    pass

class UserModel(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, autoincrement=True)
    email = Column(String(255), unique=True, nullable=False, index=True)
    name = Column(String(255), nullable=False)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, server_default="CURRENT_TIMESTAMP")

    def to_entity(self) -> User:
        return User(id=self.id, email=self.email, name=self.name, is_active=self.is_active)

    @classmethod
    def from_entity(cls, entity: User) -> "UserModel":
        return cls(id=entity.id, email=entity.email, name=entity.name, is_active=entity.is_active)
```

### Fabryka sesji

```python
from sqlalchemy.ext.asyncio import create_async_engine, async_sessionmaker, AsyncSession
from app.core.config import settings

engine = create_async_engine(settings.DATABASE_URL, echo=settings.DEBUG, pool_size=10, max_overflow=20)
async_session = async_sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)

async def get_session() -> AsyncSession:
    async with async_session() as session:
        async with session.begin():
            yield session
```

## Migracje (Alembic)

Przed utworzeniem nowej migracji:
- sprawdź istniejące pliki w `backend/alembic/versions/`
- zachowaj dokładnie tę samą konwencję numeracji i nazewnictwa, która już występuje w projekcie
- nie wprowadzaj własnego formatu nazw plików

```bash
# Wszystko przez Docker:
make db-migrate msg="add users table"    # alembic revision --autogenerate
make db-upgrade                          # alembic upgrade head
make db-downgrade                        # alembic downgrade -1
make db-history                          # alembic history
```

## Anty-wzorce (VETO)

- Raw SQL w serwisach -> użyj repository
- `session.execute()` w warstwie application -> przenieś do adaptera
- Import SQLAlchemy w domain/ -> NIGDY
- `localhost` jako DB host w kontenerze -> użyj nazwy serwisu (`mariadb`)
- Brak `expire_on_commit=False` w async session -> dodaj
- `session.commit()` w repository -> używaj `session.flush()`, commit w session factory
- Brak healthcheck na MariaDB -> dodaj, użyj `healthcheck.sh --connect --innodb_initialized`
