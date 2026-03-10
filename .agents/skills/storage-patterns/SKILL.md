---
name: storage-patterns
description: >
  Wzorce zarządzania plikami i storage. Czytaj gdy pracujesz z uploadem, generowaniem plików, dokumentami.
---

# Skill: Wzorce storage

> Wzorce zarządzania plikami i storage. Czytaj, gdy pracujesz z uploadem, generowaniem plików i dokumentami.

---

## Architektura

Storage to bind mount z hosta do kontenera:
- Host: `${STORAGE_PATH:-./storage}/` (z .env)
- Kontener: `/app/storage/`

## Hierarchia katalogów

```
storage/
  {tenant_id}/                  # izolacja per tenant/user
    uploads/                    # pliki wrzucone przez użytkownika
      {YYYY-MM}/                # partycjonowanie po dacie
        {uuid}.{ext}            # unikalna nazwa (nigdy oryginalna!)
    generated/                  # pliki wygenerowane przez system
      reports/
        {uuid}.pdf
      exports/
        {uuid}.json
    temp/                       # pliki tymczasowe (czyszczone cron/task)
      {uuid}.{ext}
```

## Zasady

1. **Nigdy nie używaj oryginalnej nazwy pliku** jako nazwy na dysku -> UUID + rozszerzenie
2. **Oryginalna nazwa** zapisana w bazie danych (kolumna `original_filename`)
3. **Partycjonowanie** po dacie (`YYYY-MM/`) zapobiega milionom plików w jednym katalogu
4. **Izolacja** per tenant/user -> `{tenant_id}/` na pierwszym poziomie
5. **Temp** czyszczony automatycznie po 24h (Huey periodic task)
6. **STORAGE_PATH** konfigurowalny przez .env -> łatwe przeniesienie na S3/GCS w przyszłości

## Model bazodanowy

```python
class StoredFile(Base):
    __tablename__ = "stored_files"

    id = Column(Integer, primary_key=True)
    uuid = Column(String(36), unique=True, nullable=False, index=True)
    tenant_id = Column(Integer, ForeignKey("users.id"), nullable=False, index=True)
    original_filename = Column(String(500), nullable=False)
    storage_path = Column(String(1000), nullable=False)  # relative to STORAGE_PATH
    mime_type = Column(String(255), nullable=False)
    size_bytes = Column(BigInteger, nullable=False)
    category = Column(String(50), nullable=False)  # uploads, generated, temp
    created_at = Column(DateTime, server_default="CURRENT_TIMESTAMP")
```

## Port (interfejs)

```python
from abc import ABC, abstractmethod

class FileStorage(ABC):
    @abstractmethod
    async def save(self, tenant_id: int, category: str, filename: str, content: bytes) -> str:
        """Zapisz plik, zwróć UUID."""
        ...

    @abstractmethod
    async def get_path(self, tenant_id: int, uuid: str) -> Path | None:
        """Zwróć ścieżkę do pliku na dysku."""
        ...

    @abstractmethod
    async def delete(self, tenant_id: int, uuid: str) -> bool:
        """Usuń plik z dysku i bazy."""
        ...
```

## Adapter (implementacja lokalna)

```python
from pathlib import Path
from uuid import uuid4
from datetime import datetime

class LocalFileStorage(FileStorage):
    def __init__(self, base_path: str = "/app/storage"):
        self._base = Path(base_path)

    async def save(self, tenant_id: int, category: str, filename: str, content: bytes) -> str:
        file_uuid = str(uuid4())
        ext = Path(filename).suffix
        date_dir = datetime.now().strftime("%Y-%m")
        rel_path = f"{tenant_id}/{category}/{date_dir}/{file_uuid}{ext}"
        full_path = self._base / rel_path
        full_path.parent.mkdir(parents=True, exist_ok=True)
        full_path.write_bytes(content)
        return file_uuid
```

## Anty-wzorce (VETO)

- Oryginalna nazwa pliku jako ścieżka -> path traversal attack
- Brak izolacji per tenant -> wycieki danych
- Pliki w katalogu aplikacji (`backend/app/files/`) -> użyj storage/
- Hardcoded ścieżka -> użyj STORAGE_PATH z .env
- Brak limitu rozmiaru uploadu -> dodaj w endpoincie
- Brak walidacji MIME type -> sprawdzaj zawartość, nie rozszerzenie
