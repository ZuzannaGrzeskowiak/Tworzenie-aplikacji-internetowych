# Przewodnik Dla Developerów - Shared Pantry Tracker

## Spis Treści

1. [Konfiguracja Środowiska](#konfiguracja-środowiska)
2. [Struktura Projektu](#struktura-projektu)
3. [Konwencje Kodowania](#konwencje-kodowania)
4. [Git Workflow](#git-workflow)
5. [Debugging i Testowanie](#debugging-i-testowanie)
6. [Performance Tips](#performance-tips)
7. [FAQ](#faq)

---

## Konfiguracja Środowiska

### Wymagania
- Node.js 16+ lub 18+
- npm 8+ lub yarn 3+
- Git
- IDE: VS Code (rekomendowane) z rozszerzeniami:
  - ES7+ React/Redux/React-Native snippets
  - Prettier - Code formatter
  - ESLint
  - TypeScript Vue Plugin

### Instalacja

1. **Klonuj repozytorium**
```bash
git clone <repository-url>
cd Break
```

2. **Zainstaluj zależności**
```bash
npm install
```

3. **Sprawdź wersję Node.js**
```bash
node --version  # powinno być >= 16.0.0
npm --version   # powinno być >= 8.0.0
```

4. **Uruchom projekt lokalnie**
```bash
npm run dev
```

5. **Otwórz w przeglądarce**
```
http://localhost:3000
```

---

## Struktura Projektu

### Katalogi i pliki

```
Break/
│
├── src/
│   ├── components/              # Komponenty React
│   │   ├── PantryList.tsx      # Główny komponent
│   │   ├── PantryList.module.css
│   │   ├── PantryItem.tsx      # Komponent karty
│   │   ├── PantryItem.module.css
│   │   ├── AddPantryForm.tsx   # Formularz
│   │   ├── AddPantryForm.module.css
│   │   ├── PantryControls.tsx  # Kontrolki
│   │   └── PantryControls.module.css
│   │
│   ├── utils/                  # Funkcje pomocnicze
│   │   ├── dateUtils.ts        # Logika dat
│   │   ├── dateUtils.test.ts   # Testy
│   │   ├── storageService.ts   # Zarządzanie stanem
│   │   └── storageService.test.ts
│   │
│   ├── App.tsx                 # Root component
│   ├── App.css
│   ├── main.tsx                # Entry point
│   ├── index.css               # Style globalne
│   └── setupTests.ts           # Konfiguracja testów
│
├── public/
│   └── vite.svg
│
├── index.html                  # HTML template
├── package.json                # Zależności
├── tsconfig.json               # Konfiguracja TypeScript
├── tsconfig.node.json
├── vite.config.ts              # Konfiguracja Vite
├── jest.config.js              # Konfiguracja Jest
├── README.md                   # Dokumentacja główna
└── DEVELOPERS.md               # Ten plik
```

### Typ każdego folderu

| Folder | Cel |
|--------|-----|
| `src/components/` | Komponenty React + style |
| `src/utils/` | Logika biznesowa, testy |
| `public/` | Zasoby statyczne |

---

## Konwencje Kodowania

### Nazewnictwo

**Komponenty React**
```typescript
// ✅ POPRAWNIE - PascalCase
export const PantryList: React.FC = () => { }
export const PantryItem: React.FC<Props> = () => { }

// ❌ BŁĘDNIE
export const pantryList = () => { }
export const pantry_list = () => { }
```

**Funkcje i zmienne**
```typescript
// ✅ POPRAWNIE - camelCase
const getDaysUntilExpiry = (date: string) => { }
const handleAddItem = () => { }

// ❌ BŁĘDNIE
const get_days_until_expiry = (date: string) => { }
const GetDaysUntilExpiry = (date: string) => { }
```

**Konstante**
```typescript
// ✅ POPRAWNIE
const STORAGE_KEY = 'pantry_items';
const MAX_QUANTITY = 999;

// ❌ BŁĘDNIE
const storage_key = 'pantry_items';
const max_quantity = 999;
```

**Interfejsy i Typy**
```typescript
// ✅ POPRAWNIE - PascalCase, "I" prefix opcjonalny
interface PantryItem { }
type FilterStatus = 'all' | 'expired' | ...

// ❌ BŁĘDNIE
interface pantryItem { }
type filterStatus = 'all' | ...
```

### Import/Export

```typescript
// ✅ POPRAWNIE - named exports dla utility'ów
export const getDaysUntilExpiry = () => { }
export interface PantryItem { }

// ✅ POPRAWNIE - default export dla komponentów
const PantryList: React.FC = () => { }
export default PantryList;

// ❌ BŁĘDNIE - default export dla utility'ów
export default function getDaysUntilExpiry() { }
```

### Formatowanie Kodu

Używamy Prettier z domyślnymi ustawieniami:

```bash
# Format all files
npm run format

# Check formatting
npm run format:check
```

### JSDoc Komentarze

```typescript
/**
 * Oblicza liczbę dni do wygaśnięcia produktu
 * @param expiryDate - Data ważności w formacie ISO
 * @returns Liczba dni (ujemna jeśli przeterminowany)
 */
export const getDaysUntilExpiry = (expiryDate: string): number => {
  // ...
}
```

### Typy w React

```typescript
// ✅ POPRAWNIE
interface PantryItemProps {
  item: PantryItem;
  onEdit: (item: PantryItem) => void;
  onDelete: (id: string) => void;
}

const PantryItemComponent: React.FC<PantryItemProps> = ({ 
  item, 
  onEdit, 
  onDelete 
}) => { }

// ❌ BŁĘDNIE
const PantryItemComponent = ({ item, onEdit, onDelete }) => { }
```

---

## Git Workflow

### Branch Strategy

Używamy Feature Branch workflow:

```
main
  ├── feature/add-notifications (Faza 2)
  ├── feature/auth-system (Faza 2)
  └── bugfix/date-parsing (hotfix)
```

### Commit Messages

Format: `type(scope): message`

```bash
# ✅ POPRAWNIE
git commit -m "feat(components): add PantryControls component"
git commit -m "fix(utils): correct date calculation logic"
git commit -m "docs: update README with API documentation"
git commit -m "test(utils): add tests for date sorting"

# ❌ BŁĘDNIE
git commit -m "update stuff"
git commit -m "fixed bug"
```

### Typy commitów

| Typ | Opis |
|-----|------|
| `feat` | Nowa funkcja |
| `fix` | Naprawa błędu |
| `docs` | Dokumentacja |
| `test` | Testy |
| `refactor` | Refaktoryzacja kodu |
| `perf` | Optymalizacja |
| `chore` | Zmiany w buildzie, zależności |

### Pull Request Template

```markdown
## Opis
Krótki opis zmian

## Typ PR
- [ ] Bug fix
- [ ] Nowa funkcja
- [ ] Breaking change
- [ ] Dokumentacja

## Checklist
- [ ] Kod przejrzany
- [ ] Testy dodane/zaktualizowane
- [ ] Dokumentacja zaktualizowana
- [ ] Brak console.log w produkcji

## Screenshots (jeśli dotyczy)
```

---

## Debugging i Testowanie

### VS Code Debug Setup

Utwórz `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Vite",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/src",
      "sourceMapPathOverride": {
        "/src/*": "${webspaceFolder}/src/*"
      }
    }
  ]
}
```

### Testowanie Lokalne

```bash
# Uruchom jeden test plik
npm test -- dateUtils.test.ts

# Testy w trybie watch
npm test -- --watch

# Testy z debuggingiem
node --inspect-brk ./node_modules/.bin/jest --runInBand

# Raport pokrycia
npm run test:coverage
```

### React DevTools

1. Zainstaluj rozszerzenie [React DevTools](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
2. Otwórz DevTools (F12)
3. Tab "Components" - inspekcja komponentów
4. Tab "Profiler" - analiza performance

### Browser DevTools Tips

```javascript
// Storage Inspector
// Application → Local Storage → http://localhost:3000
// Podgląd key "pantry_items"

// Performance
// Performance tab → Record
// Sprawdź React rendering

// Network
// Network tab
// Sprawdzanie HTTP requestów (przyszłe Fazy)
```

---

## Performance Tips

### React Optimization

```typescript
// ❌ POWOLI - Re-render'uje przy każdej zmianie
const PantryList = () => {
  const [items, setItems] = useState([])
  
  const sortedItems = items.sort() // Oblicza za każdym razem
  
  return items.map(item => <PantryItem key={item.id} />)
}

// ✅ SZYBKO - Memoizowanie
const PantryList = () => {
  const [items, setItems] = useState([])
  
  const sortedItems = useMemo(() => 
    [...items].sort((a, b) => a.expiryDate.localeCompare(b.expiryDate)),
    [items]
  )
  
  return items.map(item => <PantryItem key={item.id} item={item} />)
}
```

### Local Storage Performance

```typescript
// ❌ POWOLI - Zapisuje za każdą zmianę
items.forEach(item => {
  localStorage.setItem(`item_${item.id}`, JSON.stringify(item))
})

// ✅ SZYBKO - Jedna operacja
localStorage.setItem('pantry_items', JSON.stringify(items))
```

### CSS Performance

```css
/* ❌ POWOLI - Animacja na właściwościach kosztownych */
.item {
  transition: all 0.3s;
}

/* ✅ SZYBKO - Animacja tylko na transform i opacity */
.item {
  transition: transform 0.3s, opacity 0.3s;
}
```

### Bundle Size

```bash
# Sprawdź rozmiar bundla
npm run build

# Analiza bundla
npm install -D vite-plugin-visualizer
```

---

## FAQ

### P: Jak dodać nową kategorię produktów?

O: Edytuj `src/components/AddPantryForm.tsx`:

```typescript
const CATEGORIES = [
  'Warzywa',
  'Owoce',
  'Mleczne',
  'Mięso',
  'Ryby',
  'Chlebowe',
  'Napoje',
  'Nowa Kategoria', // ← Dodaj tutaj
  'Inne',
];
```

### P: Gdzie zmieniać kolory aplikacji?

O: Kolory zdefiniowane w CSS Modules:
- `src/components/*.module.css` - kolory komponentów
- `src/index.css` - kolory globalne (gradient na tle)

Paleta kolorów:
```
#667eea - Główny (fiolet)
#764ba2 - Gradient (ciemniejszy fiolet)
#51cf66 - Świeży (zielony)
#ffa94d - Wkrótce wygasa (pomarańczowy)
#ff6b6b - Przeterminowany (czerwony)
```

### P: Jak dodać nowe pole do produktu?

O: 
1. Rozszerz interfejs `PantryItem` w `src/utils/dateUtils.ts`
2. Dodaj pole w formularzu `src/components/AddPantryForm.tsx`
3. Dodaj renderowanie w `src/components/PantryItem.tsx`
4. Zaktualizuj testy

### P: Jak tworzyć komponenty?

O: Używaj template'u:

```typescript
import React from 'react';
import styles from './MyComponent.module.css';

interface MyComponentProps {
  title: string;
  onAction: () => void;
}

/**
 * Krótki opis komponentu
 */
const MyComponent: React.FC<MyComponentProps> = ({ title, onAction }) => {
  return (
    <div className={styles.container}>
      <h2>{title}</h2>
      <button onClick={onAction}>Action</button>
    </div>
  );
};

export default MyComponent;
```

### P: Jak testować komponenty?

O: Użyj React Testing Library:

```typescript
import { render, screen } from '@testing-library/react';
import PantryItem from '../PantryItem';

describe('PantryItem', () => {
  it('should render item name', () => {
    const mockItem = { id: '1', name: 'Milk', ... };
    render(<PantryItem item={mockItem} onEdit={() => {}} onDelete={() => {}} />);
    expect(screen.getByText('Milk')).toBeInTheDocument();
  });
});
```

### P: Jak debugować Local Storage?

O: W konsoli DevTools:

```javascript
// Podgląd wszystkich danych
JSON.parse(localStorage.getItem('pantry_items'))

// Wyczyszczenie
localStorage.clear()

// Dodanie test item'u
const testItem = { id: '1', name: 'Test', ... }
localStorage.setItem('pantry_items', JSON.stringify([testItem]))
```

### P: Jak obsługiwać błędy?

O: Twórz error boundaries i obsługuj wyjątki:

```typescript
try {
  const date = parseISO(invalidDate);
} catch (error) {
  console.error('Invalid date:', error);
  return 'Invalid date';
}
```

### P: Jak skalować projekt na Fazę 2?

O: 
1. Zainstaluj React Router: `npm install react-router-dom`
2. Utwórz Context dla autoryzacji
3. Dodaj fetch/axios dla API
4. Zmiguj Local Storage na Backend

### P: Który IDE rekomendujesz?

O: **VS Code** z rozszerzeniami:
- ES7+ React/Redux/React-Native snippets (by dsznajder)
- Prettier - Code formatter
- ESLint
- Thunder Client (do testowania API - Faza 2)

---

## Przydatne Linki

- [React Documentation](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Vite Guide](https://vitejs.dev/guide/)
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [date-fns](https://date-fns.org/)
- [MDN Web Docs](https://developer.mozilla.org/)

---

**Wersja dokumentacji**: 1.0  
**Data ostatniej aktualizacji**: Grudzień 2024  
**Autoria**: Tim Development
