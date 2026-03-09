---
name: add-frontend-page
description: Use when adding a new page to the React frontend - routing, component, state, API integration "Step-by-step guide to add a new page in the React frontend (UI template)"
---

# Skill: Add Frontend Page

Full procedure for adding a new page in the React SPA with UI template.

## 0. Check UI framework Template FIRST

> **NIGDY nie twórz UI od zera.** Sprawdź `frontend/src/components/ (istniejące)` - czy istnieje gotowy odpowiednik.

1. Przeszukaj `frontend/src/components/ (istniejące)` pod kątem gotowego komponentu/strony
2. Zaproponuj mapping: "Użyję card z X, tabelę z Y"
3. Po akceptacji - implementuj

## 1. Create Page Component (`frontend/src/features/`)

```tsx
// frontend/src/features/my-module/components/MyModulePage.tsx
import { Fragment } from 'react';
import { Container } from '@/components/container';

export function MyModulePage() {
  return (
    <Fragment>
      <Container>
        <div className="card">
          <div className="card-header">
            <h3 className="card-title">My Module</h3>
          </div>
          <div className="card-body">
            {/* Content */}
          </div>
        </div>
      </Container>
    </Fragment>
  );
}
```

## 2. Add Route (`frontend/src/routing/`)

```tsx
{
  path: 'my-module',
  element: <MyModulePage />,
}
```

## 3. Add Menu Item (`frontend/src/config/`)

```tsx
{
  title: 'My Module',
  icon: 'element-11',
  path: '/my-module',
}
```

## 4. UI framework Components - Quick Reference

| Use Case | Component / Class |
|----------|------------------|
| Page wrapper | `<Container>` |
| Card | `card`, `card-header`, `card-body` |
| Data table | `table table-border` + DataGrid |
| Button | `btn btn-primary`, `btn btn-sm btn-light` |
| Badge | `badge badge-success`, `badge badge-warning` |
| Icons | `<KeenIcon icon="..." />` |
| Modal | UI framework modal component |

## 5. Dark Mode - MANDATORY

Every element MUST support both themes:
```tsx
<div className="bg-white dark:bg-coal-600 text-gray-800 dark:text-gray-200">
```

## 6. Data Fetching Pattern

```tsx
import { useQuery } from '@tanstack/react-query';
import { apiClient } from '@/services/api-client';

export function useMyEntities() {
  return useQuery({
    queryKey: ['my-entities'],
    queryFn: () => apiClient.get('/my-entities'),
    staleTime: 5 * 60 * 1000,
  });
}
```

## 7. Internationalization (i18n)

```tsx
import { useTranslation } from 'react-i18next';

export function MyComponent() {
  const { t } = useTranslation();
  return <h1>{t('myModule.title')}</h1>;
}
```

---

## See also (MANDATORY)

- **Before implementation** - read `workflows/dt-development.md`
- **Frontend patterns** - read `skills/frontend-patterns.md`
- **Frontend role** - read `roles/frontend-engineer.md`
- **After implementation** - run `workflows/post-impl-verify.md`
