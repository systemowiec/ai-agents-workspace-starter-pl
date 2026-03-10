---
name: add-frontend-page
description: Użyj przy dodawaniu nowej strony do frontendu React - routing, komponent, state, integracja z API. "Przewodnik krok po kroku dodawania nowej strony do frontendu React (szablon UI)"
---

# Skill: Dodawanie strony frontendu

Pełna procedura dodawania nowej strony w React SPA z użyciem szablonu UI.

## 0. Najpierw sprawdź szablon UI frameworka

> **NIGDY nie twórz UI od zera.** Sprawdź `frontend/src/components/ (istniejące)` - czy istnieje gotowy odpowiednik.

1. Przeszukaj `frontend/src/components/ (istniejące)` pod kątem gotowego komponentu/strony
2. Zaproponuj mapping: "Użyję card z X, tabelę z Y"
3. Po akceptacji - implementuj

## 1. Utwórz komponent strony (`frontend/src/features/`)

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

## 2. Dodaj route (`frontend/src/routing/`)

```tsx
{
  path: 'my-module',
  element: <MyModulePage />,
}
```

## 3. Dodaj element menu (`frontend/src/config/`)

```tsx
{
  title: 'My Module',
  icon: 'element-11',
  path: '/my-module',
}
```

## 4. Komponenty UI frameworka - szybkie odniesienie

| Zastosowanie | Komponent / klasa |
|--------------|-------------------|
| Wrapper strony | `<Container>` |
| Card | `card`, `card-header`, `card-body` |
| Tabela danych | `table table-border` + DataGrid |
| Przycisk | `btn btn-primary`, `btn btn-sm btn-light` |
| Badge | `badge badge-success`, `badge badge-warning` |
| Ikony | `<KeenIcon icon="..." />` |
| Modal | komponent modala z UI frameworka |

## 5. Dark Mode - OBOWIĄZKOWY

Każdy element MUSI obsługiwać oba motywy:
```tsx
<div className="bg-white dark:bg-coal-600 text-gray-800 dark:text-gray-200">
```

## 6. Wzorzec pobierania danych

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

## Zobacz też (OBOWIĄZKOWE)

- **Przed implementacją** - przeczytaj `workflows/dt-development.md`
- **Wzorce frontendowe** - przeczytaj `skills/frontend-patterns.md`
- **Rola frontendowa** - przeczytaj `roles/frontend-engineer.md`
- **Po implementacji** - uruchom `workflows/post-impl-verify.md`
