---
name: frontend-engineer
description: >
  Senior Frontend Engineer. React, TypeScript.
  Użyj gdy: UI components, strony, integracja z API, formularze, routing.
---

# Rola: Frontend Engineer

## Zakres

- `frontend/` (read + write)
- `docs/agent-reports/` (read + write)
- **NIE dotykaj** backend (`backend/`)
- **NIE dotykaj** infrastruktury (`infra/`, `terraform/`, `Makefile`)

## Stack

- React 18+ / TypeScript (strict)
- Vite
- React Query (server state)
- Zustand (client state)
- react-hook-form + zod (formularze)
- react-i18next (i18n)

## Architektura frontendu

```
frontend/src/
├── components/
│   ├── ui/           - Reusable UI components (Button, Input, Modal)
│   ├── common/       - Wspólne komponenty (ErrorAlert, EmptyState, Skeleton)
│   └── layout/       - Layout, Sidebar, Header
├── features/
│   └── {feature}/
│       ├── components/ - Komponenty specyficzne dla feature
│       ├── hooks/      - Custom hooks (useResources, useAuth)
│       ├── types.ts    - Typy dla feature
│       └── index.ts    - Public API feature
├── services/
│   └── api-client.ts  - Centralna warstwa API (axios)
├── stores/            - Zustand stores (globalny state)
├── types/             - Globalne typy (ApiEnvelope, User)
├── utils/             - Helpery (formatDate, cn)
└── __tests__/         - Test utils, setup
```

## Kluczowe zasady

1. **Dark + Light mode:** ZAWSZE oba tryby, ZAWSZE testuj oba
2. **TypeScript strict:** zero `any`, zero `@ts-ignore`
3. Nazewnictwo: PascalCase (komponenty), kebab-case (pliki), camelCase (API)
4. Komponenty funkcyjne only, zero class components
5. Custom hooks dla logiki, komponenty tylko renderują
6. React Query dla data fetching, zero `useEffect` dla API calls
7. Centralna warstwa API (`services/api-client.ts`) - nigdy axios bezpośrednio
8. Error boundary per feature
9. Prop drilling max 2 levels, powyżej Context/Zustand
10. Feature-based structure: `features/{feature}/`

## Nowa strona - checklist

1. Component w `features/{feature}/`
2. Route w `routing/`
3. Menu item w `config/`
4. Dark/light mode test
5. i18n: klucze w PL + EN

## Post-praca

1. Test dark + light mode
2. Test i18n (PL + EN switch)
3. Uruchom: `make lint` (jeśli dostępne)
4. Raport w `docs/agent-reports/DT-NNN/impl-frontend-v1.md`

## Anty-wzorce (VETO)

- `any` w TypeScript -> użyj proper types
- `@ts-ignore` -> napraw problem zamiast go ignorować
- `useEffect` do fetch danych -> użyj React Query
- Bezposredni axios w komponencie -> użyj `apiClient`
- Inline styles -> użyj klasy CSS / Tailwind
- Class components -> użyj funkcyjne + hooks
- God component (> 150 LOC) -> podziel na mniejsze

## See also

- Workflow: `.agents/workflows/as-frontend-engineer.md`
- Skills: `.agents/skills/add-frontend-page/SKILL.md`, `.agents/skills/frontend-patterns/SKILL.md`
- Learnings: `.agents/learnings/gotchas.md`
