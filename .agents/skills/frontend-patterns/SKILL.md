---
name: frontend-patterns
description: >
  Wzorce i szablony dla frontend-dev. Czytaj gdy implementujesz nowy kod.
---

# Skill: Wzorce frontendowe

> Wzorce i szablony dla frontend-dev. Czytaj gdy implementujesz nowy kod.

---

## Wrapper Metric UI

```tsx
// components/ui/Button.tsx
import { Button as MetricButton, type ButtonProps as MetricButtonProps } from 'metric-ui';

interface ButtonProps extends MetricButtonProps {
  isLoading?: boolean;
}

export function Button({ isLoading, children, disabled, ...props }: ButtonProps) {
  return (
    <MetricButton disabled={disabled || isLoading} {...props}>
      {isLoading ? <Spinner size="sm" /> : children}
    </MetricButton>
  );
}
```

## Klient API

```tsx
// services/api-client.ts
import axios, { type AxiosInstance, type AxiosError } from 'axios';
import type { ApiEnvelope, ApiError } from '@/types/api';

class ApiClient {
  private client: AxiosInstance;

  constructor() {
    this.client = axios.create({
      baseURL: import.meta.env.VITE_API_URL ?? '/api/v1',
      timeout: 15_000,
      headers: { 'Content-Type': 'application/json' },
    });

    this.client.interceptors.request.use((config) => {
      const token = this.getToken();
      if (token) config.headers.Authorization = `Bearer ${token}`;
      return config;
    });

    this.client.interceptors.response.use(
      (res) => res,
      (err: AxiosError<ApiError>) => {
        if (err.response?.status === 401) this.handleUnauthorized();
        return Promise.reject(err);
      },
    );
  }

  async get<T>(url: string, params?: Record<string, unknown>): Promise<ApiEnvelope<T>> {
    const { data } = await this.client.get<ApiEnvelope<T>>(url, { params });
    return data;
  }

  async post<T>(url: string, body?: unknown): Promise<ApiEnvelope<T>> {
    const { data } = await this.client.post<ApiEnvelope<T>>(url, body);
    return data;
  }

  private getToken(): string | null { return null; /* implementacja */ }
  private handleUnauthorized(): void { /* redirect login */ }
}

export const apiClient = new ApiClient();
```

## Hook React Query

```tsx
// features/resources/hooks/useResources.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { apiClient } from '@/services/api-client';
import type { Resource, ResourceCreate } from '../types';

const QUERY_KEY = ['resources'] as const;

export function useResources(params?: { cursor?: string; limit?: number }) {
  return useQuery({
    queryKey: [...QUERY_KEY, params],
    queryFn: () => apiClient.get<Resource[]>('/resources', params),
    staleTime: 5 * 60 * 1000,
  });
}

export function useCreateResource() {
  const qc = useQueryClient();
  return useMutation({
    mutationFn: (payload: ResourceCreate) => apiClient.post<Resource>('/resources', payload),
    onSuccess: () => qc.invalidateQueries({ queryKey: QUERY_KEY }),
  });
}
```

## Komponent feature'a

```tsx
// features/resources/components/ResourceList.tsx
import { useResources } from '../hooks/useResources';
import { EmptyState } from '@/components/common/EmptyState';
import { ErrorAlert } from '@/components/common/ErrorAlert';
import { Skeleton } from '@/components/ui/Skeleton';

export function ResourceList() {
  const { data, isLoading, isError, error } = useResources();

  if (isLoading) return <ResourceListSkeleton />;
  if (isError) return <ErrorAlert error={error} />;
  if (!data?.data.length) return <EmptyState message="Brak zasobów" />;

  return (
    <div className="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
      {data.data.map((resource) => (
        <ResourceCard key={resource.id} resource={resource} />
      ))}
    </div>
  );
}
```

## Narzędzia testowe

```tsx
// __tests__/test-utils.tsx
import { render, type RenderOptions } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { MemoryRouter } from 'react-router-dom';

export function renderWithProviders(
  ui: React.ReactElement,
  options?: RenderOptions & { route?: string },
) {
  const qc = new QueryClient({
    defaultOptions: { queries: { retry: false }, mutations: { retry: false } },
  });
  const { route = '/', ...rest } = options ?? {};

  return {
    ...render(ui, {
      wrapper: ({ children }) => (
        <QueryClientProvider client={qc}>
          <MemoryRouter initialEntries={[route]}>{children}</MemoryRouter>
        </QueryClientProvider>
      ),
      ...rest,
    }),
    queryClient: qc,
  };
}
```

## tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "jsx": "react-jsx",
    "baseUrl": ".",
    "paths": { "@/*": ["src/*"] }
  },
  "include": ["src"]
}
```

## Zależności baseline

```json
{
  "dependencies": {
    "react": "^18.3", "react-dom": "^18.3", "react-router-dom": "^6.28",
    "@tanstack/react-query": "^5.60",
    "react-hook-form": "^7.53", "zod": "^3.23", "@hookform/resolvers": "^3.9",
    "axios": "^1.7", "zustand": "^5.0"
  },
  "devDependencies": {
    "typescript": "^5.7", "@types/react": "^18.3",
    "vitest": "^2.1", "@testing-library/react": "^16.0",
    "@testing-library/user-event": "^14.5", "msw": "^2.6",
    "eslint": "^9.15", "prettier": "^3.4"
  }
}
```
