# Frontend

> Ten katalog jest celowo pusty. Jest częścią starter kita.

## Dla agenta

Kiedy użytkownik poprosi o stworzenie frontendu:

1. **Przeczytaj** `AGENTS.md` (root) - architektura, reguły, Cardinal Rules
2. **Przeczytaj** `.agents/context/project-overview.md` - stack technologiczny
3. **Wezwij Platform Engineera** (`/as-platform-engineer`) do:
   - Stworzenia `infra/docker-compose.yml` z serwisem frontend
   - Stworzenia `infra/frontend/Dockerfile` (multi-stage)
   - Dodania targetów do `Makefile`
4. **Dopiero potem** inicjalizuj frontend w tym katalogu

## Oczekiwana struktura (React SPA)

```
frontend/
├── src/
│   ├── components/          # UI components (ui/, common/, layout/)
│   ├── features/            # Feature-based modules
│   ├── services/            # API client
│   ├── stores/              # State management (Zustand/Redux)
│   ├── types/               # Global TypeScript types
│   └── utils/               # Helpers
└── public/                  # Static assets
```

## Skille do użycia

- `.agents/skills/frontend-patterns/SKILL.md`
- `.agents/skills/add-frontend-page/SKILL.md`
