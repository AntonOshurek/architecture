# props-drilling.md

# Props Drilling – Dlaczego tego unikać?

Props drilling to przekazywanie propsów przez wiele komponentów pośrednich tylko po to, aby dotarły do komponentu docelowego.

## 🔴 Dlaczego to zły wzorzec?

- Powoduje **nadmierne sprzężenie komponentów**.
- Zwiększa trudność refaktoryzacji.
- Zmniejsza czytelność kodu i utrudnia jego utrzymanie.

## ❌ Przykład props drilling

```
App
└── Parent
    └── Child
        └── Profile
```

➡️ Wartość `userName` przechodzi przez **Parent → Child → Profile**, mimo że Parent i Child jej nie potrzebują.

```tsx
// App.tsx
import { Parent } from './Parent';

export const App = () => {
  return <Parent userName="Janek" />;
};

// Parent.tsx
import { Child } from './Child';

export const Parent = ({ userName }: { userName: string }) => {
  return <Child userName={userName} />;
};

// Child.tsx
import { Profile } from './Profile';

export const Child = ({ userName }: { userName: string }) => {
  return <Profile userName={userName} />;
};

// Profile.tsx
export const Profile = ({ userName }: { userName: string }) => {
  return <div>Cześć, {userName}</div>;
};
```

---

## ✅ Jak zrobić to lepiej?

### 🟢 Schemat z Context API

```
App (UserProvider)
└── Parent
    └── Child
        └── Profile (useUser)
```

➡️ Komponenty pośrednie nie muszą znać `userName`, bo Profile pobiera ją bezpośrednio z Context.

---
