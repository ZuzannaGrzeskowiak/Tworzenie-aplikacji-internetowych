# Getting Started - Szybki Start

## Dla Impatientów ⚡

```bash
# 1. Zainstaluj zależności
npm install

# 2. Uruchom aplikację
npm run dev

# 3. Otwórz http://localhost:3000
```

## Kroki Instalacji 📋

### 1. Wymagania Wstępne

Zainstaluj:
- [Node.js](https://nodejs.org/) (wersja 16+)
- [Git](https://git-scm.com/)
- IDE: [VS Code](https://code.visualstudio.com/) (rekomendowane)

Sprawdź instalację:
```bash
node --version    # powinno być v16.0.0 lub wyżej
npm --version     # powinno być 8.0.0 lub wyżej
git --version
```

### 2. Klonowanie Repozytorium

```bash
git clone <url-repozytorium>
cd Break
```

### 3. Instalacja Zależności

```bash
npm install
```

To zainstaluje wszystkie wymagane paczki z `package.json`.

### 4. Uruchomienie Serwera Deweloperskiego

```bash
npm run dev
```

Terminal pokaże:
```
  VITE v5.0.8  ready in XXX ms

  ➜  Local:   http://localhost:3000/
  ➜  press h + enter to show help
```

### 5. Otwórz w Przeglądarce

Kliknij na [http://localhost:3000](http://localhost:3000) lub wpisz w pasku adresu.

---

## Dostępne Komendy 🚀

| Komenda | Opis |
|---------|------|
| `npm run dev` | Uruchom serwer deweloperski |
| `npm run build` | Buduj dla produkcji |
| `npm run preview` | Preview buildu produkcyjnego |
| `npm test` | Uruchom testy |
| `npm run test:coverage` | Testy z raportem pokrycia |

---

## Pierwsze Kroki w Aplikacji 🎯

1. **Zaloguj się** (jeszcze niedostępne w MVP, będzie w Fazie 2)
2. **Dodaj produkt** - kliknij "+ Dodaj nowy produkt"
3. **Wypełnij formularz**:
   - Nazwa: np. "Mleko"
   - Ilość: np. "1"
   - Jednostka: np. "l"
   - Kategoria: "Mleczne"
   - Data ważności: np. "15.12.2024"
4. **Kliknij "Dodaj produkt"**
5. **Przejrzyj listę** - produkty będą automatycznie sortowane

---

## Struktura Projektu 📁

```
Break/
├── src/                    # Kod źródłowy
│   ├── components/        # Komponenty React
│   ├── utils/            # Funkcje pomocnicze
│   └── App.tsx           # Główny komponent
├── public/               # Zasoby statyczne
├── index.html            # HTML template
├── package.json          # Zależności
├── vite.config.ts        # Konfiguracja Vite
├── jest.config.js        # Konfiguracja testów
├── README.md             # Dokumentacja
├── DEVELOPERS.md         # Przewodnik dla developerów
└── ROADMAP.md           # Plan rozwoju
```

---

## Dokumentacja 📚

- **README.md** - Pełna dokumentacja aplikacji
- **DEVELOPERS.md** - Przewodnik dla programistów
- **ROADMAP.md** - Plan rozwoju (Fazy 2-3)

---

## Rozwiązywanie Problemów 🔧

### Problem: "Command not found: npm"

**Rozwiązanie**: Zainstaluj [Node.js](https://nodejs.org/)

### Problem: "Port 3000 already in use"

**Rozwiązanie**: 
```bash
# Linux/Mac
lsof -i :3000
kill -9 <PID>

# Windows PowerShell
Get-Process -Id (Get-NetTCPConnection -LocalPort 3000).OwningProcess | Stop-Process -Force
```

### Problem: "Cannot find module..."

**Rozwiązanie**: 
```bash
rm -rf node_modules package-lock.json
npm install
```

### Problem: Aplikacja nie ładuje się

**Rozwiązanie**:
1. Sprawdź konsolę DevTools (F12)
2. Sprawdź czy serwer Vite jest uruchomiony
3. Wyczyść cache przeglądarki (Ctrl+Shift+Delete)

---

## VS Code Setup (Rekomendowany) 🎨

### Rozszerzenia

Zainstaluj:
1. **ES7+ React/Redux/React-Native snippets** (by dsznajder)
2. **Prettier - Code formatter**
3. **ESLint**
4. **Thunder Client** (do testowania API w przyszłości)

### VS Code Settings

Utwórz `.vscode/settings.json`:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

---

## Git Basics 🐙

```bash
# Status zmian
git status

# Dodaj zmian
git add .

# Commit
git commit -m "feat: add new feature"

# Push
git push origin main

# Pull
git pull origin main

# Utwórz nowy branch
git checkout -b feature/my-feature
```

---

## Testowanie Aplikacji ✅

```bash
# Uruchom testy
npm test

# Testy w trybie watch (auto-rerun)
npm test -- --watch

# Raport pokrycia
npm run test:coverage

# Jeden test file
npm test -- dateUtils.test.ts
```

---

## Browser DevTools 🔍

### Local Storage Inspector

1. Otwórz DevTools (F12)
2. Idź do **Application** tab
3. Z lewej strony: **Local Storage** → **http://localhost:3000**
4. Szukaj klucza `pantry_items`

### React DevTools

1. Zainstaluj [React Developer Tools](https://chromewebstore.google.com/detail/react-developer-tools)
2. Otwórz DevTools (F12)
3. Przejdź do **Components** tab
4. Inspekcja komponentów w real-time

---

## Performance Monitoring 📊

```bash
# Build size analysis
npm run build

# Preview
npm run preview
```

Check `dist/` folder dla rozmiarów plików.

---

## Cheat Sheet - Najczęściej Używane Komendy

```bash
# Start development
npm run dev

# Run tests
npm test

# Build production
npm run build

# Install new package
npm install package-name

# Remove package
npm uninstall package-name

# Check outdated packages
npm outdated

# Update all packages
npm update
```

---

## Zasoby Online

- 📖 [React Docs](https://react.dev)
- 📖 [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- 📖 [Vite Documentation](https://vitejs.dev/)
- 📖 [date-fns](https://date-fns.org/)
- 📖 [Jest Testing Framework](https://jestjs.io/)

---

## Wsparcie i Pytania ❓

- **GitHub Issues** - Zgłaszanie bugów
- **Discussions** - Pytania i dyskusje
- **Email**: support@example.com (w przyszłości)

---

## Co Dalej? 🚀

Po zaznajomieniu się z aplikacją:

1. Przeczytaj **README.md** dla pełnego przeglądu
2. Przeczytaj **DEVELOPERS.md** jeśli chcesz zmienić kod
3. Sprawdź **ROADMAP.md** dla planu przyszłości
4. Zaproponuj ulepszenia poprzez Pull Requests

---

**Miłego kodowania! 💻**

*Ostatnia aktualizacja: Grudzień 2024*
