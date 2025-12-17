# 📦 Shared Pantry Tracker - Wspólny Magazyn/Spiżarnia

## Przegląd projektu

Aplikacja webowa do zarządzania wspólnymi zasobami spiżarni, lodówki lub magazynu. Umożliwia współlokatorom, rodzinom lub pracownikom biura śledzenie produktów, ich ilości i dat ważności w celu zmniejszenia marnowania żywności i optymalizacji zakupów.

### Główne cele

- ✅ Umożliwienie wielu użytkownikom jednoczesnego dodawania, edytowania i usuwania produktów
- 📅 Monitorowanie dat ważności z automatycznym oznaczaniem przeterminowanych produktów
- 📊 Sortowanie i filtrowanie produktów po dacie ważności, nazwie i kategorii
- 💾 Trwałe przechowywanie danych w przeglądarce (Local Storage)
- 📱 Responsywny interfejs dostosowany do urządzeń mobilnych

---

## Architektura projektu

### Tech Stack

- **Frontend**: React 18 + TypeScript
- **Build Tool**: Vite
- **Styling**: CSS Modules
- **Date Management**: date-fns
- **Testing**: Jest + React Testing Library
- **Local Storage**: Browser API

### Struktura folderów

```
Break/
├── src/
│   ├── components/
│   │   ├── PantryList.tsx          # Główny komponent
│   │   ├── PantryItem.tsx          # Komponent karty produktu
│   │   ├── AddPantryForm.tsx       # Formularz dodawania/edycji
│   │   ├── PantryControls.tsx      # Filtry i sortowanie
│   │   └── *.module.css            # Style CSS Modules
│   ├── utils/
│   │   ├── dateUtils.ts            # Logika obsługi dat
│   │   ├── storageService.ts       # Zarządzanie Local Storage
│   │   └── *.test.ts               # Testy jednostkowe
│   ├── App.tsx                     # Główny komponent aplikacji
│   ├── main.tsx                    # Entry point
│   └── index.css                   # Style globalne
├── public/
│   └── vite.svg                    # Logo
├── package.json                    # Zależności projektu
├── tsconfig.json                   # Konfiguracja TypeScript
├── vite.config.ts                  # Konfiguracja Vite
├── jest.config.js                  # Konfiguracja Jest
├── index.html                      # HTML template
└── README.md                       # Ta dokumentacja
```

---

## Fazy Rozwoju

### FAZA 1: MVP (Minimum Viable Product) ✅ AKTUALNIE REALIZOWANA

**Status**: W trakcie realizacji

#### Wymagania funkcjonalne:
1. **Interfejs responsywny**
   - Forma do dodawania produktów
   - Lista wyświetlających produkty
   - Kontrolki sortowania i filtrowania
   - Automatyczne destacenie produktów przeterminowanych

2. **Zarządzanie produktami**
   - Dodawanie nowych produktów (nazwa, ilość, jednostka, data ważności, kategoria)
   - Edycja istniejących produktów
   - Usuwanie produktów
   - Przechowywanie w Local Storage

3. **Logika dat ważności**
   - Prawidłowe porównywanie dat
   - Statusy: "świeży" (>3 dni), "wkrótce wygasa" (≤3 dni), "przeterminowany" (<0 dni)
   - Wyświetlanie liczby dni do wygaśnięcia

4. **Sortowanie i filtrowanie**
   - Sortowanie po dacie ważności (domyślnie)
   - Sortowanie po nazwie produktu
   - Sortowanie po kategorii
   - Filtrowanie po statusie (wszystko, świeże, wkrótce wygasa, przeterminowane)

#### Komponenty React:
- `PantryList` - główny komponent do zarządzania stanem aplikacji
- `AddPantryForm` - formularz dodawania/edycji produktów
- `PantryItem` - pojedynczy element listy
- `PantryControls` - kontrolki sortowania, filtrowania i statystyki

#### Fokus React:
- `useState` - zarządzanie stanem
- `useEffect` - ładowanie danych z Local Storage
- Warunkowe renderowanie - wyświetlanie statusów

---

### FAZA 2: Wersja 1.0 (Pełna Funkcjonalność)

**Status**: Zaplanowana

#### Nowe wymagania:
1. **Autoryzacja**
   - System rejestracji/logowania
   - Walidacja e-maila
   - Bezpieczne przechowywanie haseł

2. **Współdzielenie**
   - Możliwość tworzenia grup/magazynów
   - Zapraszanie użytkowników do grupy
   - Uprawnienia do edycji (właściciel, edytor, przeglądający)

3. **Powiadomienia**
   - Alerty o produktach wygasających w ciągu 3 dni
   - Powiadomienia desktop (Push Notifications)
   - Historia zmian

4. **Rozszerzenia filtrowania**
   - Wyszukiwanie po nazwie
   - Filtrowanie po dacie dodania
   - Zaawansowane filtry

#### Fokus React:
- React Router DOM - nawigacja multi-page
- Context API - zarządzanie stanem globalnym
- Custom Hooks - logika wspólna
- Asynchroniczne operacje (fetch/Axios)

---

### FAZA 3: Przyszłe Rozszerzenia

**Status**: Koncepcja

#### Planowane funkcje:
1. **Lista Zakupów** - generowanie automatyczne na bazie brakujących produktów
2. **Skaner Kodów** - integracja z kamerą PWA
3. **Wykresy i Statystyki** - wizualizacja marnowania żywności
4. **Integracja z API** - zsynchronizowanie z chmurą
5. **Powiadomienia e-mail** - cykliczne raporty

---

## Podręcznik Instalacji i Uruchomienia

### Wymagania wstępne
- Node.js (wersja 16+)
- npm lub yarn

### Kroki instalacji

1. **Instalacja zależności**
```bash
npm install
```

2. **Uruchomienie serwera deweloperskiego**
```bash
npm run dev
```
Aplikacja będzie dostępna pod adresem `http://localhost:3000`

3. **Budowanie dla produkcji**
```bash
npm run build
```

4. **Preview produkcyjnego buildu**
```bash
npm run preview
```

5. **Uruchomienie testów**
```bash
npm test
```

6. **Uruchomienie testów z raportem pokrycia**
```bash
npm run test:coverage
```

---

## Komponenty i Interfejsy

### PantryItem (Interfejs)

```typescript
interface PantryItem {
  id: string;                // Unikalny identyfikator
  name: string;              // Nazwa produktu
  quantity: number;          // Ilość
  unit: string;              // Jednostka (szt., kg, l, itp.)
  expiryDate: string;        // Data ważności (ISO format)
  category: string;          // Kategoria
  createdAt: string;         // Data utworzenia
}
```

### Kategorie produktów

- Warzywa
- Owoce
- Mleczne
- Mięso
- Ryby
- Chlebowe
- Napoje
- Inne

### Statusy produktów

- **✅ Świeży** - data ważności > 3 dni
- **⚠️ Wkrótce wygasa** - data ważności ≤ 3 dni
- **❌ Przeterminowany** - data ważności upłynęła

---

## Funkcjonalności

### Dodawanie produktu

1. Kliknij przycisk "+ Dodaj nowy produkt"
2. Wypełnij formularz:
   - Nazwa produktu *
   - Ilość *
   - Jednostka
   - Kategoria
   - Data ważności *
3. Kliknij "Dodaj produkt"

**UX Target**: Maksymalnie 3 kliknięcia/dotknięcia na urządzeniu mobilnym ✅

### Edycja produktu

1. Kliknij przycisk "Edytuj" na karcie produktu
2. Zmodyfikuj potrzebne pola
3. Kliknij "Zaktualizuj"

### Usuwanie produktu

1. Kliknij przycisk "Usuń" na karcie produktu
2. Potwierdź usunięcie

### Filtrowanie

Używaj listy rozwijalnej "Filtruj" aby wyświetlić:
- Wszystkie produkty
- Tylko świeże
- Tylko wkrótce wygasające
- Tylko przeterminowane

### Sortowanie

Użyj listy rozwijalnej "Sortuj" aby posortować po:
- Dacie ważności (domyślnie)
- Nazwie produktu (alfabetycznie)
- Kategorii

### Statystyki

Panel kontrolny wyświetla:
- **Razem** - liczba wszystkich produktów
- **Przeterminowane** - liczba produktów przeterminowanych
- **Wkrótce wygasa** - liczba produktów wygasających w ciągu 3 dni

---

## Funkcje Utility

### dateUtils.ts

#### `getDaysUntilExpiry(expiryDate: string): number`
Oblicza liczbę dni do wygaśnięcia produktu.

#### `getItemStatus(expiryDate: string): FilterStatus`
Zwraca status produktu ('fresh', 'expiring-soon', 'expired').

#### `formatDate(dateString: string): string`
Formatuje datę na format DD.MM.YYYY.

#### `isValidExpiryDate(dateString: string): boolean`
Waliduje, czy data jest w przyszłości i poprawnie sformatowana.

#### `sortPantryItems(items: PantryItem[], sortBy: SortBy): PantryItem[]`
Sortuje produkty po wybranym kluczu.

#### `filterPantryItems(items: PantryItem[], status: FilterStatus): PantryItem[]`
Filtruje produkty po statusie.

#### `generateId(): string`
Generuje unikalny ID dla nowego produktu.

### storageService.ts

#### `getItems(): PantryItem[]`
Pobiera wszystkie produkty z Local Storage.

#### `setItems(items: PantryItem[]): void`
Zapisuje produkty do Local Storage.

#### `addItem(item: PantryItem): PantryItem[]`
Dodaje nowy produkt i zwraca zaktualizowaną listę.

#### `updateItem(id: string, updates: Partial<PantryItem>): PantryItem[]`
Aktualizuje istniejący produkt.

#### `deleteItem(id: string): PantryItem[]`
Usuwa produkt i zwraca zaktualizowaną listę.

#### `clearAll(): void`
Usuwa wszystkie produkty z Local Storage.

---

## Testowanie

### Pokrycie testami (Target: 80%)

Projekt jest wyposażony w testy jednostkowe dla:

1. **dateUtils.test.ts** - Logika obsługi dat
   - Obliczanie dni do wygaśnięcia
   - Determinacja statusu produktu
   - Formatowanie dat
   - Validacja dat
   - Sortowanie produktów
   - Filtrowanie produktów
   - Generowanie ID

2. **storageService.test.ts** - Zarządzanie Local Storage
   - Pobieranie produktów
   - Dodawanie produktów
   - Aktualizacja produktów
   - Usuwanie produktów
   - Czyszczenie storage

### Uruchomienie testów

```bash
# Uruchom wszystkie testy
npm test

# Uruchom testy z raportem pokrycia
npm run test:coverage

# Uruchom testy w trybie watch
npm test -- --watch
```

---

## Dostępność (Accessibility)

Aplikacja jest dostosowana do osób niepełnosprawnych:

- ✅ Właściwe etykiety dla wszystkich pól formularza
- ✅ Atrybuty `aria-label` na przyciskach
- ✅ Kontrast kolorów zgodny ze standardem WCAG AA
- ✅ Nawigacja klawiatury (Tab, Enter)
- ✅ Wskaźniki fokusa
- ✅ Semantyczne elementy HTML

---

## Stylowanie - CSS Modules

Projekt używa CSS Modules do unikania kolizji stylów:

```
src/components/
├── PantryList.module.css
├── PantryItem.module.css
├── AddPantryForm.module.css
└── PantryControls.module.css
```

Każdy moduł jest scoped do danego komponentu, co gwarantuje brak globalnych kolizji.

### Paleta kolorów

- **Główny gradient**: #667eea → #764ba2 (fiolet)
- **Zielony (świeży)**: #51cf66
- **Pomarańczowy (wkrótce wygasa)**: #ffa94d
- **Czerwony (przeterminowany)**: #ff6b6b
- **Neutralny (tło)**: #f0f0f0

---

## Responsywny Design

Aplikacja jest w pełni responsywna:

- 📱 Mobile-first podejście
- 📱 Breakpoint: 600px
- 🖥️ Desktop: optymalizacja dla szerszych ekranów
- ⌚ Touch-friendly interfejs (obszary klikalne 48x48px minimum)

---

## Local Storage

Aplikacja przechowuje dane w Local Storage:

```javascript
Key: "pantry_items"
Value: JSON Array of PantryItem objects
```

**Wielkoć maksymalna**: ~5-10MB (zależy od przeglądarki)

Wszystkie dane są przechowywane lokalnie i nie są wysyłane na serwer.

---

## Cele Programisty - Faza 1.0

### Cele Techniczne (Twarde)

- ✅ **Zarządzanie Datami**: Prawidłowe parsowanie, przechowywanie i porównywanie dat ważności (date-fns)
- ✅ **Architektura Komponentów**: Hierarchia PantryList → PantryItem
- ✅ **Optymalizacja**: Przygotowanie do lazy loading i React.Suspense
- ⏳ **Integracja z Backendem**: Przygotowana struktura do API REST (Faza 2)

### Cele Projektowe (Miękkie)

- ✅ **UX**: Dodawanie produktu w maksymalnie 3 kliknięciach
- ✅ **Testowanie**: Pokrycie testami ~80%+ dla logiki dat i sortowania
- ✅ **Dostępność**: Etykiety, atrybuty aria, kontrast kolorów
- ✅ **CSS Modules**: Modułowość stylów bez kolizji

---

## Znane Ograniczenia

- Local Storage ma limit rozmiaru (~5-10MB)
- Brak synchronizacji między kartami przeglądarki (Faza 2)
- Brak kopii zapasowej (Faza 2)
- Brak autoryzacji (Faza 2)
- Brak współdzielenia między użytkownikami (Faza 2)

---

## Plany na przyszłość

### Krótkoterminowe (Faza 2)
- [ ] Backend API (Node.js/Express lub Python/Django)
- [ ] Baza danych (PostgreSQL/MongoDB)
- [ ] Autoryzacja (JWT)
- [ ] Wsparcie dla wielu użytkowników
- [ ] Powiadomienia push

### Średnioterminowe (Faza 3)
- [ ] PWA - offline support
- [ ] Skaner kodów kreskowych
- [ ] Wykresy i statystyki
- [ ] Generowanie listy zakupów
- [ ] Integracja z API supermarketów

---

## Contributing

Aby przyczynić się do projektu:

1. Fork repozytorium
2. Stwórz branch feature (`git checkout -b feature/AmazingFeature`)
3. Commit zmian (`git commit -m 'Add some AmazingFeature'`)
4. Push do brancha (`git push origin feature/AmazingFeature`)
5. Otwórz Pull Request

---

## Licencja

Projekt udostępniony na licencji MIT.

---

## Autor

Stworzono jako część kursu "Tworzenie aplikacji internetowych" - Kognitywistyka Komunikacji, II stopień.

**Data utworzenia**: Grudzień 2024

---

## Kontakt i Wsparcie

W razie pytań lub problemów prosimy o zgłoszenie issue w repozytorium.

---

## Zasoby i Dokumentacja

- [React Documentation](https://react.dev)
- [Vite Guide](https://vitejs.dev)
- [date-fns Documentation](https://date-fns.org)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Jest Testing Framework](https://jestjs.io)
- [CSS Modules](https://github.com/css-modules/css-modules)
