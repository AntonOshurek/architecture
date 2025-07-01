# api-structure.md

# Architektura API z CQRS
[CQRS pattern (Microsoft Docs)](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)

Poniżej znajduje się struktura plików dla Twojego projektu z wykorzystaniem **CQRS pattern** oraz podziału konfiguracji API:

✅ globalna konfiguracja w `shared` ✅ osobne konfiguracje i requesty w `entities/user` i `entities/document`

---

## 📂 Struktura plików

```
src/
├── shared/
│   └── api/
│       ├── client.ts        ← globalna konfiguracja axios/fetch
│       └── config.ts        ← np. baseURL, interceptors
├── entities/
│   ├── user/
│   │   ├── api/
│   │   │   ├── queries.ts   ← zapytania GET (Query)
│   │   │   └── commands.ts  ← mutacje POST/PUT/DELETE (Command)
│   │   └── model/
│   │       └── types.ts
│   └── document/
│       ├── api/
│       │   ├── queries.ts   ← zapytania GET (Query)
│       │   └── commands.ts  ← mutacje POST/PUT/DELETE (Command)
│       └── model/
│           └── types.ts
```

---

## 🔑 Kluczowe założenia CQRS w projekcie

1. **Queries (zapytania)** oddzielone od **Commands (mutacje)**:

   - ułatwia rozdzielenie odpowiedzialności,
   - poprawia czytelność kodu.

2. **shared/api/client.ts**

   - globalny axios instance lub fetch wrapper z interceptorami, np. auth token.

3. **entities/user/api/** oraz **entities/document/api/**

   - zawierają funkcje wykorzystujące client z shared, z podziałem na `queries.ts` i `commands.ts`.

---

## 📝 Przykład (user/api/queries.ts)

```ts
import { client } from '@/shared/api/client';

export const getCurrentUser = async () => {
  const response = await client.get('/user/me');
  return response.data;
};
```

---

## 📝 Przykład pliku `.env`

```
VITE_API_URL=https://api.example.com
```

---

## 📝 Przykład config.ts

```ts
export const CONFIG = {
  API_URL: import.meta.env.VITE_API_URL,
};
```

---

## 📝 Przykład client.ts (axios)

```ts
import axios from 'axios';
import { CONFIG } from './config';

export const client = axios.create({
  baseURL: CONFIG.API_URL,
  withCredentials: true,
});
```

---
