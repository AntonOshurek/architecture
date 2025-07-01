# public-api-detailed.md

# Public API – szczegółowe wyjaśnienie z przykładami importów i komunikacją segmentów

## 🔑 Definicja

**Public API** to kontrakt między grupą modułów (np. slice) a kodem, który z nich korzysta. Działa jako brama, umożliwiając dostęp tylko do wybranych obiektów i tylko przez ten publiczny API.

---

## ✅ Dobry przykład importu w FSD (features + entities)

### Struktura

```
src/
├── features/
│   └── user-create/
│       ├── index.ts
│       ├── model/
│       │   └── useCreateUser.ts
│       └── ui/
│           └── CreateUserForm.tsx
└── entities/
    └── user/
        ├── index.ts
        ├── model/
        │   ├── index.ts
        │   └── types.ts
        ├── store/
        │   └── store.ts
        └── ui/
            └── UserForm.tsx
```

---

### 🔄 Diagram przepływu importów

```
features/user-create/index.ts
├── export { useCreateUser } from './model/useCreateUser';
└── export { CreateUserForm } from './ui/CreateUserForm';

features/user-create/model/useCreateUser.ts
└── import { userStore } from '@entities/user';

entities/user/index.ts
├── export * from './store/store';
├── export * from './model';
└── export * from './ui/UserForm';

entities/user/model/index.ts
└── export * from './types';

entities/user/ui/UserForm.tsx
└── import { User } from '../model';
```

➡️ **Opis:**

- **features/user-create/model/useCreateUser.ts** importuje store из **entities/user** przez public API `@entities/user`.
- **entities/user/ui/UserForm.tsx** importuje typ **User** z `model/index.ts` public API внутри slice.
- **features/user-create/index.ts** eksportuje swój model i ui jako public API feature.

---

### Przykład komunikacji między segmentami slice

#### entities/user/ui/UserForm.tsx

```ts
import { User } from '../model'; // import przez public API modelu

export const UserForm = ({ user }: { user: User }) => {
  return <div>{user.name}</div>;
};
```

➡️ Tutaj **ui** importuje z **model** przez public API `model/index.ts`, zachowując изоляцию сегментов.

---

## ❌ Zły przykład (łamie public API rule)

```ts
import { User } from '../model/types'; // ZABRONIONE – bezpośredni import z pliku
```

---

## 🔑 Kluczowe korzyści

- Chroni przed zmianami struktury segmentów.
- Ułatwia рефакторинг i понимание API slice.
- Zachowuje spójność importów w całym проекте.

---
