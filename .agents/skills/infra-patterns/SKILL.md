---
name: infra-patterns
description: >
  Wzorce i szablony dla infra-dev. Czytaj gdy konfigurujesz Docker, CI/CD, serwery.
---

# Skill: Wzorce infrastrukturalne

> Wzorce i szablony dla infra-dev. Czytaj gdy konfigurujesz Docker, CI/CD, serwery.

---

## Szablon serwisu w `docker-compose.yml`

```yaml
services:
  my-service:
    build:
      context: ..
      dockerfile: infra/my-service/Dockerfile
      target: development
    container_name: ${PROJECT_NAME:-app}-my-service
    restart: unless-stopped
    ports:
      - "${SERVICE_PORT:-8080}:8080"
    volumes:
      - ../backend/app:/app/app:cached
    environment:
      - DATABASE_URL=mysql+asyncmy://${DB_USER}:${DB_PASSWORD}@mariadb:3306/${DB_NAME}
      - REDIS_URL=redis://redis:6379/0
    depends_on:
      mariadb:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "python", "-c", "import httpx; httpx.get('http://localhost:8080/health')"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s
    networks:
      - app-network
    mem_limit: 512m
    cpus: 1.0
```

## Szablon wieloetapowego Dockerfile

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /build
COPY infra/backend/requirements.txt ./
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Development (hot reload, narzędzia debugowania)
FROM python:3.12-slim AS development
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app
COPY --from=builder /install /usr/local
COPY backend/ ./
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]

# Production (minimalny obraz, bez narzędzi developerskich)
FROM python:3.12-slim AS production
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app
COPY --from=builder /install /usr/local
COPY backend/app/ ./app/
USER appuser
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
    CMD python -c "import httpx; httpx.get('http://localhost:8000/health')"
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--workers", "4"]
```

## Szablon targetu Makefile

```makefile
.PHONY: my-target
my-target: ## Opis targetu
	$(COMPOSE) exec backend <komenda>
```

## Polityka requirements

Starter używa jednego `requirements.txt`. Gdy dochodzi CI/CD lub wiele środowisk:

1. Rozdziel na `requirements/base.txt`, `requirements/dev.txt`, `requirements/prod.txt`
2. `dev.txt` i `prod.txt` zaczynają się od `-r base.txt`
3. Kontenery produkcyjne instalują TYLKO `prod.txt` (lekkie, single-responsibility)
4. Dev kontenery instalują `dev.txt` (pytest, ruff, mypy, factory-boy)
5. Dockerfile stages: `development` -> `dev.txt`, `production` -> `prod.txt`
6. NIGDY nie instaluj narzędzi testowych na produkcji

## Dobre praktyki `.env`

```bash
# Unikalne porty per projekt (unikaj kolizji)
PROJECT_NAME=my-app
BACKEND_PORT=8050
DB_PORT=3350
REDIS_PORT=6350

# Unikalne nazwy volume'ów i sieci (automatycznie z PROJECT_NAME)
# ${PROJECT_NAME}-mariadb-data, ${PROJECT_NAME}-network

# Sekrety - NIGDY nie commituj `.env`, wygeneruj:
# openssl rand -hex 32
SECRET_KEY=<wygenerowany>
```

## Wzorce healthchecków

```yaml
# MariaDB
healthcheck:
  test: ["CMD", "healthcheck.sh", "--connect", "--innodb_initialized"]
  interval: 10s
  timeout: 5s
  retries: 5
  start_period: 30s

# Redis
healthcheck:
  test: ["CMD", "redis-cli", "ping"]
  interval: 10s
  timeout: 3s
  retries: 5

# FastAPI
healthcheck:
  test: ["CMD", "python", "-c", "import httpx; httpx.get('http://localhost:8000/health')"]
  interval: 30s
  timeout: 5s
  retries: 3
  start_period: 10s
```

## Mikroserwis - szablon niezależnego serwisu

Każdy mikroserwis ma własny Dockerfile, compose, requirements:

```
services/
  my-service/
    app/
      main.py
      config.py
    tests/
    infra/
      Dockerfile
      docker-compose.yml
      requirements.txt
    pyproject.toml
```

Mikroserwis musi być całkowicie niezależny - można go skopiować do innego projektu.
