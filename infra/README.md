# Infrastructure

> Ten katalog jest celowo pusty. Jest częścią starter kita.

## Dla agenta

Kiedy użytkownik poprosi o konfiguracje infrastruktury:

1. **Przeczytaj** `AGENTS.md` (root) - Docker First, Cardinal Rules
2. **Przeczytaj** `.agents/context/project-overview.md` - stack technologiczny
3. **Aktywuj role** Platform Engineer (`/as-platform-engineer`)
4. **Stwórz** konfiguracje w tym katalogu

## Oczekiwana struktura

```
infra/
├── docker-compose.yml       # Container orchestration
├── docker-compose.dev.yml   # Overrides for development
├── backend/
│   ├── Dockerfile            # Multi-stage (builder -> dev -> prod)
│   └── requirements.txt      # Zależności TYLKO dla backendu
├── frontend/
│   └── Dockerfile            # Multi-stage
├── worker/
│   ├── Dockerfile            # Osobny kontener = osobne requirements
│   └── requirements.txt      # Zależności TYLKO dla workera
├── nginx/                    # Reverse proxy (production)
│   └── nginx.conf
└── storage/                  # Volume mounts (gitignored)
```

## Kluczowe zasady (Platform Engineer)

### Docker First
- NIGDY nie uruchamiaj komend lokalnie (`python`, `pip`, `npm`, `npx`)
- Wszystko przez `make` lub `docker compose exec`
- Każdy build, test i migracja odbywa się w kontenerze

### Requirements per kontener
- **NIGDY nie rób uniwersalnego requirements.txt** dla całego projektu
- Każdy kontener (backend, worker, frontend, scheduler) ma OSOBNY plik zależności
- Kontenery muszą być lekkie - minimum bibliotek potrzebnych do działania
- Jeśli worker nie potrzebuje FastAPI - nie instaluj FastAPI w workerze
- Jeśli backend nie potrzebuje Celery - nie instaluj Celery w backendzie

### Pozostałe zasady
- Multi-stage Dockerfiles (builder -> development -> production)
- Health checks na każdym serwisie
- Non-root user w kontenerach produkcyjnych
- Resource limits (mem_limit, cpus) zdefiniowane
- Bazą danych na prywatnej sieci Docker (brak portów na hoście)

## Skille do użycia

- `.agents/skills/infra-patterns/SKILL.md`
