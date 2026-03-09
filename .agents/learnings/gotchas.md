# Gotchas

Pułapki i nietypowe zachowania odkryte podczas pracy nad projektem.
Agent może proponować nowe wpisy - zawsze po akceptacji użytkownika.

---

<!-- Przykładowy wpis (usuń po dodaniu pierwszego prawdziwego):

### asyncmy wymaga jawnego zamknięcia sesji
**Kontekst**: backend, SQLAlchemy async z asyncmy driver
**Problem**: Połączenia do DB wiszą po zakończeniu requestu
**Rozwiązanie**: Zawsze używaj `async with session:` lub jawne `await session.close()`
**Data**: 2026-01-15

-->
