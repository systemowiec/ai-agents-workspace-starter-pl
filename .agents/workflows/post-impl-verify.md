---
description: Obowiązkowa weryfikacja po każdej zmianie w kodzie - smoke testy, testy jednostkowe, logi kontenerów. Uruchom PRZED poinformowaniem użytkownika, że praca jest zakończona.
---

# Weryfikacja po implementacji

> **OBOWIĄZKOWA po KAŻDEJ zmianie - backend, frontend, config, Docker, routing, infrastruktura.**
> Agent NIE może zgłosić, że praca jest zakończona, bez wykonania WSZYSTKICH kroków poniżej.
> Jeśli którykolwiek krok nie powiedzie się - napraw problem i powtórz procedurę od początku.

---

## Kiedy uruchamiać

**ZAWSZE** - po zakończeniu implementacji każdego feature'a, bugfixa, refaktoru lub zmiany konfiguracyjnej.
Dotyczy zarówno zmian backendowych, jak i frontendowych.

---

## Krok 1: Określ zakres

| Zmiana w...                     | Serwisy do weryfikacji    |
| ------------------------------- | ------------------------- |
| `backend/`                      | kontener backendu         |
| `frontend/`                     | kontener frontendu        |
| `infra/`                        | WSZYSTKIE kontenery       |
| `docker-compose*.yml`           | WSZYSTKIE kontenery       |
| migracjach                      | kontener backendu (DB)    |

---

## Krok 2: Przebuduj zmieniony serwis

```bash
make build
make up
```

Odczekaj 5-10 sekund na start usług, a następnie przejdź do kroku 3.

---

## Krok 3: Sprawdzenie zdrowia kontenerów

```bash
make status
```

**WYMAGANE:** Wszystkie serwisy mają status `Up (healthy)` albo `Up`. Żaden nie może mieć statusu `Exited`.
Jeśli którykolwiek ma status `Exited` - przeczytaj logi (krok 4), napraw problem i wróć do kroku 2.

---

## Krok 4: Sprawdzenie logów (bez ERROR/EXCEPTION)

```bash
make logs
```

**SZUKAJ:** `ERROR`, `Exception`, `Traceback`, `Fatal`, `CRITICAL`.
Jeśli coś znajdziesz - napraw problem i wróć do kroku 2.

---

## Krok 4b: Celowana weryfikacja logów kontenerów (OBOWIĄZKOWA dla backendu)

> **Po KAZDEJ zmianie backendowej agent MUSI sprawdzic logi KAZDEGO zmienionego kontenera indywidualnie.**
> `make logs` moze nie wychwycic wszystkiego - sprawdz konkretne serwisy.

```bash
# Sprawdz logi backendu (ostatnie 50 linii):
docker compose -f infra/docker-compose.yml logs --tail=50 backend

# Sprawdz logi database-gateway (jesli zmieniony):
docker compose -f infra/docker-compose.yml logs --tail=50 database-gateway

# Sprawdz logi workera (jesli zmieniony):
docker compose -f infra/docker-compose.yml logs --tail=50 huey
```

**CO WERYFIKOWAC:**
- Czy kontener uruchomil sie poprawnie (brak ImportError, ModuleNotFoundError)
- Czy nie ma bledow przy starcie aplikacji (FastAPI startup, Uvicorn)
- Czy polaczenie z baza danych jest aktywne (brak ConnectionRefusedError)
- Czy nie ma nowych WARNING/ERROR po ostatniej zmianie

**ABSOLUTE RULE:** Agent NIE moze powiedziec "zrobione" bez przeczytania logow zmienionego kontenera.

---

## Krok 5: Smoke test kluczowych endpointów

```bash
# Health check backendu
docker compose -f infra/docker-compose.yml exec backend curl -sf http://localhost:8000/health | head -c 200
```

---

## Krok 6: Uruchom testy

### Backend:
```bash
make test
```

### Frontend (jeśli był zmieniany):
```bash
make lint
```

### Weryfikacja migracji (jeśli zmienił się model):
```bash
make db-upgrade
```

**WYMAGANE:** Żaden test nie może zakończyć się wynikiem FAIL. Jeśli test nie przejdzie -> napraw problem i powtórz procedurę.

---

## Krok 7: Podsumowanie weryfikacji

Zanim poinformujesz użytkownika, że praca jest zakończona, potwierdź:

| Pytanie                                     | Wymagana odpowiedź |
| ------------------------------------------- | ------------------ |
| Czy kontenery działają? (`make status`)     | Wszystkie `Up`     |
| Czy logi są czyste? (bez ERROR/Fatal)       | Tak                |
| Czy smoke testy przechodzą?                 | Tak                |
| Czy testy jednostkowe przechodzą?           | Tak (lub N/A)      |
| Czy TypeScript się kompiluje? (dla frontendu) | Tak (lub N/A)    |

**Jeśli na którekolwiek pytanie odpowiadasz "Nie" -> NIE zgłaszaj zakończenia pracy. Napraw problem i powtórz cały cykl.**

---

## Zasada absolutna

> **Agent NIGDY nie mówi użytkownikowi "zrobione", "zaimplementowane" ani "działa" bez przejścia całego tego workflowu.**
> To odpowiednik "wdrażania na produkcję bez testów" - niedopuszczalne.
