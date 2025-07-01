# public-api-cross-import.md

# Public API dla cross-importów (krosowych importów)

## 🔑 Definicja

Cross-importy są potrzebne, gdy jeden slice (np. entity) używa innego slice na tym samym poziomie (np. entities → entities), zachowując przy tym izolację i kontrakt między nimi.

---

## ✅ Zasada

- **Nie wolno** importować wewnętrznych plików slice B w slice A.
- **Można** używać specjalnego public API do cross-importów – tzw. `@x` notation.

---

## 📂 Struktura

```
src/
└── entities/
    ├── user/
    │   ├── index.ts
    │   └── @x/
    │       └── document.ts
    └── document/
        ├── index.ts
        └── model/
            └── types.ts
```

---

## 🔄 Przykład cross-importu

### entities/user/@x/document.ts

```ts
// public API tylko dla entities/document
export { getUserDisplayName } from '../lib/getUserDisplayName';
```

➡️ Tutaj **user/@x/document.ts** eksportuje funkcję tylko dla slice **document**.

---

### entities/document/model/useDocument.ts

```ts
import { getUserDisplayName } from '@entities/user/@x/document';

export const useDocument = (userId: string) => {
  const name = getUserDisplayName(userId);
  return { name };
};
```

➡️ **entities/document** importuje tylko z `@x/document`, a nie z wewnętrznych plików user.

---

## 🎯 Dlaczego?

- Pozwala na bezpieczne relacje między slice na tym samym layer.
- Chroni public API główne przed zależnościami specyficznymi dla одного slice.
- Zmniejsza coupling i zachowuje czytelność архитектуры.

---

## ⚠️ Ważne

- **Cross-imports stosuj tylko, gdy to konieczne.**
- Najczęściej używa się ich w `entities`, gdzie domenowe obiekty są powiązane biznesowo.

---
