# ⚡ QUICK REFERENCE - Shared Pantry Tracker

## 🚀 Uruchomienie (2 minuty)

```bash
npm install              # Zainstaluj zależności
npm run dev              # Uruchom aplikację
# Otwórz http://localhost:3000
```

## 📖 Dokumentacja

```
START             → GETTING_STARTED.md
UŻYTKOWNIK        → README.md
PROGRAMISTA       → DEVELOPERS.md
ARCHITEKTURA      → ARCHITECTURE.md
PLAN ROZWOJU      → ROADMAP.md
PODSUMOWANIE      → PROJECT_SUMMARY.md
KOMPLETNA LISTA   → FILE_LIST.md
ORIENTACJA        → INDEX.md
STATUS            → COMPLETION_REPORT.md
```

## 💻 Komendy

| Komenda | Opis |
|---------|------|
| `npm run dev` | Uruchom dev server |
| `npm run build` | Build do produkcji |
| `npm test` | Uruchom testy |
| `npm run test:coverage` | Raport pokrycia |
| `npm run preview` | Preview buildu |

## 📁 Struktura

```
src/
├── components/       (4 komponenty React)
├── utils/           (2 utility'i + testy)
├── App.tsx
└── index.css
```

## 🧪 Testy

```bash
npm test                    # Wszystkie testy
npm test -- --watch        # Obserwuj zmiany
npm run test:coverage      # Raport pokrycia
npm test -- dateUtils      # Jeden plik
```

## 🎯 Główne Funkcje

- ✅ Dodaj/edytuj/usuń produkty
- ✅ Monitoruj daty ważności
- ✅ Sortuj (data/nazwa/kategoria)
- ✅ Filtruj (status produktu)
- ✅ Statystyki na żywo
- ✅ Local Storage persistence

## 🛠️ Technologia

- React 18 + TypeScript
- Vite (build tool)
- CSS Modules
- date-fns
- Jest + RTL
- Local Storage API

## 🔍 Debugowanie

**Local Storage Inspector:**
1. DevTools (F12)
2. Application → Local Storage
3. Szukaj `pantry_items`

**React DevTools:**
1. Zainstaluj extension
2. DevTools (F12) → Components tab
3. Inspekcja komponentów

## 👨‍💻 Konwencje Kodowania

```typescript
// Komponenty - PascalCase
const PantryList: React.FC = () => { }

// Funkcje - camelCase
const getDaysUntilExpiry = () => { }

// Stałe - UPPER_CASE
const STORAGE_KEY = 'pantry_items'

// Interfejsy - PascalCase
interface PantryItem { }
```

## 📊 Metryki

- **Test Coverage**: 80%+
- **Bundle Size**: <200KB
- **Build Time**: ~2s
- **Lighthouse**: >90

## 🎓 React Concepts

| Concept | Gdzie |
|---------|-------|
| useState | PantryList.tsx |
| useEffect | PantryList.tsx (load storage) |
| Props | Wszystkie komponenty |
| Conditional Rendering | PantryItem.tsx (status) |
| Lists & Keys | PantryList.tsx |
| Event Handlers | AddPantryForm.tsx |

## 📦 Pakiety

```json
{
  "react": "^18.2.0",
  "typescript": "^5.2.2",
  "vite": "^5.0.8",
  "date-fns": "^2.30.0",
  "jest": "^29.7.0"
}
```

## 🔧 Konfiguracja

| Plik | Dla |
|------|-----|
| `package.json` | Zależności |
| `tsconfig.json` | TypeScript |
| `vite.config.ts` | Build |
| `jest.config.js` | Testy |

## 🌐 Browser Support

- Chrome/Edge: ✅
- Firefox: ✅
- Safari: ✅
- Mobile: ✅

## 💾 Storage

**Key**: `pantry_items`  
**Type**: JSON Array  
**Max Size**: ~5-10MB  
**Persistence**: Permanent

## 📱 Responsive

- Mobile: <600px
- Tablet: 600-1024px
- Desktop: >1024px

## 🎨 Kolory

- Primary: #667eea (fiolet)
- Fresh: #51cf66 (zielony)
- Expiring: #ffa94d (pomarańczowy)
- Expired: #ff6b6b (czerwony)

## ⌨️ Keyboard Shortcuts

| Klawisz | Akcja |
|---------|-------|
| Tab | Nawigacja |
| Enter | Submission |
| Escape | Cancel |
| F12 | DevTools |

## 🐛 Troubleshooting

| Problem | Rozwiązanie |
|---------|-----------|
| "Port 3000 in use" | Zmień port lub kill proces |
| "Module not found" | `npm install` |
| "Tests fail" | `npm test -- --watch` |
| "Build error" | Sprawdź TypeScript errors |

## 📚 Ważne Linki

- [React Docs](https://react.dev)
- [TypeScript](https://www.typescriptlang.org)
- [Vite](https://vitejs.dev)
- [Jest](https://jestjs.io)
- [date-fns](https://date-fns.org)

## 🚀 Production Deploy

```bash
npm run build              # Zbuduj
npm run preview            # Preview
# Upload 'dist/' na serwer
```

## 📞 Support

- 📖 README.md
- 🔧 DEVELOPERS.md
- 🏗️ ARCHITECTURE.md

## ✅ Pre-Commit Checklist

- [ ] `npm test` - testy pass
- [ ] `npm run build` - build success
- [ ] TypeScript - no errors
- [ ] Dokumentacja - updated

## 🎯 Next Steps

1. **Zainstaluj**: `npm install`
2. **Uruchom**: `npm run dev`
3. **Testuj**: `npm test`
4. **Czytaj**: README.md
5. **Koduj**: Dodaj nowe funkcje

---

**Version**: 1.0  
**Last Updated**: Grudzień 2024  
**Status**: ✅ Ready

---

> **Szybkiego startu! ⚡**
