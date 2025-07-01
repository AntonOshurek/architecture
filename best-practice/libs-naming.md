# libs-naming.md

# Nazewnictwo folderu `libs` w shared – podejście semantyczne

## ❌ Zły wzorzec

Rozbijanie bibliotek po **typie**, np.:

```
shared/libs/hooks/
shared/libs/utils/
shared/libs/helpers/
```

➡️ Jest to zbyt abstrakcyjne i nie mówi **do czego służy** dana biblioteka.

---

## ✅ Dobry wzorzec – nazwy po znaczeniu (purpose first)

Rozbijaj według **przeznaczenia**, np.:

```
shared/libs/
│
├── form/          ← hooki i utils do zarządzania formularzami
│   └── useFormValidation.ts
│
├── date/          ← date utils, np. formatDate, parseDate
│   └── formatDate.ts
│
├── permissions/   ← checkers i guards do ról i uprawnień
│   └── canEditDocument.ts
│
├── api/           ← wrappery fetch/axios, interceptory itp.
│   └── client.ts
│
├── storage/       ← localStorage, sessionStorage helpers
│   └── saveToLocalStorage.ts
│
└── files/         ← parsowanie plików, base64 utils itp.
    └── parsePdf.ts
```

---

## 🔑 Kluczowa zasada

**Nie nazywaj po typie (hooks, utils), tylko po przeznaczeniu (date, form, permissions).**

➡️ Dzięki temu struktura od razu mówi:

- **co robi kod** (formatowanie daty)
- **gdzie go szukać** (w date/)
- **nie obchodzi nas**, czy to hook, funkcja czy helper.

---
