# state-structure.md

# Struktura plików – Globalny i Lokalny State

Poniżej znajduje się struktura plików dla aplikacji w stylu Feature-Sliced Design (FSD) z podziałem na dwie encje: **user** i **document**.

Każda z tych encji posiada własny `store.ts`, który w kontekście encji jest **lokalnym state**, ale w kontekście całej aplikacji jest **globalnym state**.

## 📂 Struktura plików

```
src/
├── entities/
│   ├── user/
│   │   ├── index.ts
│   │   ├── model/
│   │   │   └── types.ts
│   │   ├── store/
│   │   │   └── store.ts    ← globalny state dla encji user
│   │   ├── ui/
│   │   │   └── UserProfile.tsx
│   │   └── lib/
│   │       └── utils.ts
│   └── document/
│       ├── index.ts
│       ├── model/
│       │   └── types.ts
│       ├── store/
│       │   └── store.ts    ← globalny state dla encji document
│       ├── ui/
│       │   └── DocumentViewer.tsx
│       └── lib/
│           └── utils.ts
├── shared/
│   ├── api/
│   ├── config/
│   └── ui/
├── app/
│   ├── store.ts   ← root store aplikacji (łączy store encji)
│   └── App.tsx
└── index.tsx
```

## 🔑 Kluczowe założenia

1. **`store.ts` w `entities/user` i `entities/document`:**
   - Przechowuje state związany tylko z daną encją, np.:
     - `currentUser`, `authStatus` dla user
     - `currentDocument`, `documentList` dla document

2. **`app/store.ts`:**
   - Łączy state encji w jeden root store (np. przy użyciu Redux Toolkit lub Zustand z providerem).

3. W przyszłości:
   - **features** będą korzystać ze state w `entities`.
   - Lokalne state (np. useState/useReducer) będą umieszczane w `features` lub `widgets` dla logiki UI.
