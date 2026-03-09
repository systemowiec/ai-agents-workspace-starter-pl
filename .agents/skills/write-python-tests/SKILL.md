---
name: write-python-tests
description: Use when writing or updating Python tests - pytest, factory-boy, test isolation, edge cases "Step-by-step guide to write and run Python tests (pytest + factory-boy)"
---

# Skill: Write Python Tests

## Unit Tests (`backend/app/tests/unit/`)

Test services in isolation (mock repositories):

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

## Integration Tests (`backend/app/tests/integration/`)

Test API endpoints with httpx + TestClient:

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

## Factories (`backend/app/tests/factories.py`)

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

## Running Tests

```bash
# ALL tests
make test

# Verbose output
docker compose -f infra/docker-compose.yml exec backend pytest -v --tb=short

# Specific file
docker compose -f infra/docker-compose.yml exec backend pytest app/tests/unit/test_resource_service.py

# Specific test
docker compose -f infra/docker-compose.yml exec backend pytest -k "test_create_resource_success"

# With coverage
docker compose -f infra/docker-compose.yml exec backend pytest --cov=app --cov-report=term-missing
```

## TDD Flow (Recommended)

1. **RED** - Write failing test
2. **GREEN** - Write minimum code to pass
3. **REFACTOR** - Clean up, optimize

## Rules

- Tests MUST run in Docker (NEVER locally)
- Each service method requires unit tests
- Each API endpoint requires integration test
- Use `factory-boy` for test data (NOT fixtures)
- Test edge cases: null, empty, boundary, error
- Naming: `test_<co>_<scenariusz>` or `test_<co>_<scenariusz>_<oczekiwanie>`

---

## See also (MANDATORY)

- **Before implementation** - read `workflows/dt-development.md`
- **After implementation** - run `workflows/post-impl-verify.md`
