---
name: write-python-tests
description: Użyj przy pisaniu lub aktualizacji testów Pythona - pytest, factory-boy, izolacja testów, edge cases. "Przewodnik krok po kroku pisania i uruchamiania testów Pythona (pytest + factory-boy)"
---

# Skill: Pisanie testów Python

## Testy jednostkowe (`backend/app/tests/unit/`)

Testuj serwisy w izolacji (mockowane repozytoria):

```python
# tests/unit/test_resource_service.py
import pytest
from unittest.mock import AsyncMock
from app.application.resource_service import ResourceService
from app.domain.entities import Resource
from app.domain.exceptions import NotFoundError

@pytest.fixture
def mock_repo():
    return AsyncMock()

@pytest.fixture
def service(mock_repo):
    return ResourceService(repo=mock_repo)

@pytest.mark.asyncio
async def test_create_resource_success(service, mock_repo):
    mock_repo.save.return_value = Resource(id=1, name="Test")

    result = await service.create(ResourceCreate(name="Test"))

    assert result.name == "Test"
    mock_repo.save.assert_called_once()

@pytest.mark.asyncio
async def test_get_nonexistent_resource_raises(service, mock_repo):
    mock_repo.get_by_id.return_value = None

    with pytest.raises(NotFoundError):
        await service.get_by_id(999)
```

## Testy integracyjne (`backend/app/tests/integration/`)

Testuj endpointy API przy użyciu `httpx` + `TestClient`:

```python
# tests/integration/test_resource_api.py
import pytest
from httpx import AsyncClient
from app.main import app

@pytest.mark.asyncio
async def test_list_resources():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.get("/api/v1/resources")

    assert response.status_code == 200
    assert "data" in response.json()

@pytest.mark.asyncio
async def test_create_resource():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post("/api/v1/resources", json={"name": "New"})

    assert response.status_code == 201

@pytest.mark.asyncio
async def test_unauthenticated_returns_401():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.get("/api/v1/resources")

    assert response.status_code == 401
```

## Fabryki (`backend/app/tests/factories.py`)

```python
import factory
from app.domain.entities import Resource, User

class UserFactory(factory.Factory):
    class Meta:
        model = User
    id = factory.Sequence(lambda n: n + 1)
    email = factory.LazyAttribute(lambda o: f"user{o.id}@test.com")
    name = factory.Sequence(lambda n: f"User {n}")

class ResourceFactory(factory.Factory):
    class Meta:
        model = Resource
    id = factory.Sequence(lambda n: n + 1)
    name = factory.Sequence(lambda n: f"Resource {n}")
    is_active = True
```

## Uruchamianie testów

```bash
# Wszystkie testy
make test

# Szczegółowy output
docker compose -f infra/docker-compose.yml exec backend pytest -v --tb=short

# Konkretny plik
docker compose -f infra/docker-compose.yml exec backend pytest app/tests/unit/test_resource_service.py

# Konkretny test
docker compose -f infra/docker-compose.yml exec backend pytest -k "test_create_resource_success"

# Z coverage
docker compose -f infra/docker-compose.yml exec backend pytest --cov=app --cov-report=term-missing
```

## Przepływ TDD (zalecany)

1. **RED** - napisz test, który nie przechodzi
2. **GREEN** - dopisz minimalny kod potrzebny do przejścia testu
3. **REFACTOR** - uporządkuj i zoptymalizuj rozwiązanie

## Zasady

- Testy MUSZĄ działać w Dockerze (NIGDY lokalnie)
- Każda metoda serwisowa wymaga testów jednostkowych
- Każdy endpoint API wymaga testu integracyjnego
- Do danych testowych używaj `factory-boy` (NIE fixtures)
- Testuj edge cases: null, empty, boundary, error
- Naming: `test_<co>_<scenariusz>` or `test_<co>_<scenariusz>_<oczekiwanie>`

---

## Zobacz też (OBOWIĄZKOWE)

- **Przed implementacją** - przeczytaj `workflows/dt-development.md`
- **Po implementacji** - uruchom `workflows/post-impl-verify.md`
