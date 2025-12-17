# System Architecture - Shared Pantry Tracker

## Spis Treści

1. [Overview](#overview)
2. [Component Architecture](#component-architecture)
3. [Data Flow](#data-flow)
4. [State Management](#state-management)
5. [Storage Architecture](#storage-architecture)
6. [Styling Architecture](#styling-architecture)
7. [Testing Architecture](#testing-architecture)
8. [Future Architecture (Phase 2-3)](#future-architecture)

---

## Overview

### Architektura Wysokopoziomowa

```
┌─────────────────────────────────────────────────────────┐
│                   Browser Application                   │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │              React Component Tree                │   │
│  │                                                  │   │
│  │              ┌──────────────┐                    │   │
│  │              │   App.tsx    │                    │   │
│  │              └──────┬───────┘                    │   │
│  │                     │                            │   │
│  │        ┌────────────┴────────────┐               │   │
│  │        │                         │               │   │
│  │    ┌───▼──────┐           ┌─────▼───┐           │   │
│  │    │PantryList│           │          │           │   │
│  │    └───┬──────┘           │          │           │   │
│  │        │                  │          │           │   │
│  │    ┌───┴────┬─────────┬───┴──┐       │           │   │
│  │    │        │         │      │       │           │   │
│  │ PantryItem Add Form Controls │       │           │   │
│  │                              │       │           │   │
│  └──────────────────────────────┼───────┼──────────┘   │
│                                 │       │               │
│  ┌──────────────────────────────┼───────┼──────────┐   │
│  │           Utils & Services   │       │          │   │
│  │                              │       │          │   │
│  │  dateUtils.ts ────────────────►      │          │   │
│  │  storageService.ts ─────────────────►          │   │
│  │                                                  │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │           Local Storage / Browser APIs           │   │
│  │                                                  │   │
│  │  localStorage.getItem('pantry_items')            │   │
│  │                                                  │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Warstwa Technologiczna

```
┌─────────────────────────────────┐
│   Presentation Layer            │
│   React Components + CSS Modules│
├─────────────────────────────────┤
│   Business Logic Layer          │
│   dateUtils, filterUtils        │
├─────────────────────────────────┤
│   Data Access Layer             │
│   storageService (CRUD ops)     │
├─────────────────────────────────┤
│   Storage Layer                 │
│   Browser Local Storage         │
└─────────────────────────────────┘
```

---

## Component Architecture

### Hierarchia Komponentów

```
App.tsx
  └── PantryList.tsx
      ├── AddPantryForm.tsx
      │   ├── Form inputs
      │   └── Validation logic
      │
      ├── PantryControls.tsx
      │   ├── Filter dropdown
      │   ├── Sort dropdown
      │   └── Statistics panel
      │
      └── PantryItem.tsx (mapped)
          ├── Item header
          ├── Item details
          └── Action buttons
```

### Component Responsibilities

| Komponent | Zodpowiedzialność |
|-----------|------------------|
| `App` | Root component, render tree |
| `PantryList` | State management, data fetching, orchestration |
| `AddPantryForm` | Form input, validation, submission |
| `PantryItem` | Item display, edit/delete triggers |
| `PantryControls` | Filter/sort logic, statistics display |

### Component Props Flow

```
PantryList (State)
  │
  ├─► items: PantryItem[]
  │
  ├─► AddPantryForm
  │   └─► onSubmit: (item) => void
  │
  ├─► PantryControls
  │   ├─► itemCount, expiredCount, expiringCount
  │   ├─► onSortChange, onFilterChange
  │   └─► currentSort, currentFilter
  │
  └─► PantryItem[] (mapped)
      ├─► item: PantryItem
      ├─► onEdit: (item) => void
      └─► onDelete: (id) => void
```

---

## Data Flow

### Event Flow Diagram

```
User Action (e.g., Add Item)
  │
  ▼
┌─────────────────────────────┐
│  Component Event Handler    │
│  (handleAddItem)            │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│  Call storageService        │
│  (storageService.addItem)   │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│  Update Local Storage       │
│  (localStorage.setItem)     │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│  Return updated items[]     │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│  Update Component State      │
│  setItems(updatedItems)     │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│  Re-render Components       │
│  (new items display)        │
└─────────────────────────────┘
```

### Detailed Add Item Flow

```
1. User clicks "Dodaj nowy produkt"
   └─► PantryList: setIsFormOpen(true)

2. AddPantryForm renders with input fields

3. User fills form and clicks "Dodaj produkt"
   └─► AddPantryForm: handleSubmit()

4. Validation check
   ├─► If invalid: alert() → return
   └─► If valid: proceed

5. Create item object
   ```typescript
   const item: PantryItem = {
     id: generateId(),
     name, quantity, unit, expiryDate, category,
     createdAt: new Date().toISOString()
   }
   ```

6. Call handler: onSubmit(item)
   └─► PantryList: handleAddItem(item)

7. Persist to storage
   └─► storageService.addItem(item)

8. Update state
   └─► setItems(updatedItems)

9. Re-render with new item
   └─► Item appears in list

10. Close form
    └─► setIsFormOpen(false)
```

### Edit Item Flow

```
1. User clicks "Edytuj" button
   └─► PantryList: handleEditItem(item)

2. Set editing state
   └─► setEditingItem(item)
   └─► setIsFormOpen(true)

3. Form renders with pre-filled values
   └─► useEffect in AddPantryForm

4. User modifies fields

5. Submit form
   └─► handleSubmit()

6. Check if editing
   └─► If editingItem exists:
       - Call storageService.updateItem()
       - Update item in storage

7. Update state
   └─► setItems(updatedItems)

8. Clear editing state
   └─► setEditingItem(null)
   └─► setIsFormOpen(false)
```

---

## State Management

### State Structure (MVP)

```typescript
// PantryList State
{
  items: PantryItem[],              // All items from storage
  sortBy: 'expiry' | 'name' | 'category',
  filterStatus: 'all' | 'fresh' | 'expiring-soon' | 'expired',
  editingItem: PantryItem | null,   // Currently editing item
  isFormOpen: boolean               // Show/hide form
}
```

### State Mutations

| Operation | Handler | Effect |
|-----------|---------|--------|
| Add | `handleAddItem()` | setItems(newItems) |
| Edit | `handleEditItem()` | setEditingItem(), setIsFormOpen() |
| Update | `handleAddItem()` (edit mode) | setItems(updatedItems) |
| Delete | `handleDeleteItem()` | setItems(filteredItems) |
| Sort | `setSortBy()` | Re-sort displayed items |
| Filter | `setFilterStatus()` | Re-filter displayed items |

### useEffect Usage

```typescript
// Effect: Load items from storage on mount
useEffect(() => {
  const savedItems = storageService.getItems();
  setItems(savedItems);
}, []); // Empty dependency = run once on mount
```

---

## Storage Architecture

### Local Storage Schema

```javascript
// Key: "pantry_items"
// Value: JSON array of items

[
  {
    id: "1702020000000-abc123def456",
    name: "Milk",
    quantity: 1,
    unit: "l",
    expiryDate: "2024-12-25T23:59:59.000Z",
    category: "Mleczne",
    createdAt: "2024-12-07T10:00:00.000Z"
  },
  ...
]
```

### Storage Operations

```typescript
// GET
const items = JSON.parse(localStorage.getItem('pantry_items') || '[]')

// SET
localStorage.setItem('pantry_items', JSON.stringify(items))

// DELETE
localStorage.removeItem('pantry_items')

// CLEAR
localStorage.clear()
```

### Storage Service Interface

```typescript
interface IStorageService {
  getItems(): PantryItem[];
  setItems(items: PantryItem[]): void;
  addItem(item: PantryItem): PantryItem[];
  updateItem(id: string, updates: Partial<PantryItem>): PantryItem[];
  deleteItem(id: string): PantryItem[];
  clearAll(): void;
}
```

### Data Persistence Flow

```
User Action
  │
  ▼
Component State Update
  │
  ▼
storageService.X(item)
  │
  ▼
localStorage.setItem()
  │
  ▼
Browser Persists to Disk
  │
  ▼
Data Survives Browser Close/Reload
```

---

## Styling Architecture

### CSS Modules Structure

```
Each component has its own CSS module:

PantryList.tsx ◄─► PantryList.module.css
PantryItem.tsx ◄─► PantryItem.module.css
AddPantryForm.tsx ◄─► AddPantryForm.module.css
PantryControls.tsx ◄─► PantryControls.module.css
```

### Global Styles

```
index.css
  ├── Reset (margin, padding, box-sizing)
  ├── Base styles (body, fonts)
  ├── Typography (headings, text)
  ├── Form elements (input, button, select)
  └── Utility classes
```

### Styling Approach

```typescript
// Component
import styles from './PantryItem.module.css';

export const PantryItem = () => {
  return (
    <div className={styles.itemCard}>
      <h3 className={styles.itemName}>Title</h3>
      <button className={styles.deleteBtn}>Delete</button>
    </div>
  );
};

// CSS Module - scoped automatically
.itemCard { /* Only for this component */ }
.itemName { /* Only for this component */ }
.deleteBtn { /* Only for this component */ }
```

### Color System

```css
/* Primary Brand */
--primary: #667eea
--primary-dark: #5568d3

/* Status Colors */
--fresh: #51cf66        /* Green */
--expiring: #ffa94d     /* Orange */
--expired: #ff6b6b      /* Red */

/* Neutral */
--text-dark: #333
--text-light: #666
--border: #e0e0e0
--bg-light: #f0f0f0
```

### Responsive Breakpoints

```css
/* Mobile-first approach */
/* Default: < 600px (mobile) */

/* Tablet and up */
@media (min-width: 768px) {
  /* Tablet styles */
}

/* Desktop and up */
@media (min-width: 1024px) {
  /* Desktop styles */
}
```

---

## Testing Architecture

### Test Organization

```
src/
├── utils/
│   ├── dateUtils.ts
│   └── dateUtils.test.ts      ◄─ Tests in same folder
│
├── services/
│   ├── storageService.ts
│   └── storageService.test.ts ◄─ Tests in same folder
│
└── components/
    └── __tests__/
        ├── PantryList.test.tsx
        └── AddPantryForm.test.tsx
```

### Test Coverage Target

```
utils/dateUtils.ts       - 90%+ coverage
utils/storageService.ts  - 85%+ coverage
components/              - 60%+ coverage
Overall                  - 80%+ coverage
```

### Test Types

| Type | Example | Tool |
|------|---------|------|
| Unit | dateUtils functions | Jest |
| Integration | Component + service | Jest + RTL |
| E2E | Full user flow | Cypress (future) |

### Example Unit Test

```typescript
describe('dateUtils', () => {
  describe('getDaysUntilExpiry', () => {
    it('should calculate days correctly', () => {
      const tomorrow = new Date();
      tomorrow.setDate(tomorrow.getDate() + 5);
      
      const result = getDaysUntilExpiry(tomorrow.toISOString());
      
      expect(result).toBe(5);
    });
  });
});
```

### Test Execution

```bash
# All tests
npm test

# Watch mode
npm test -- --watch

# Coverage report
npm run test:coverage

# Single file
npm test -- dateUtils.test.ts
```

---

## Future Architecture (Phase 2-3)

### Phase 2: Backend Integration

```
┌─────────────────────────────────────┐
│    Frontend (React + TypeScript)    │
│         (Current Phase 1)           │
├─────────────────────────────────────┤
│  HTTP Layer (Axios/Fetch)           │
│  Authentication (JWT)               │
│  Error Handling                     │
├─────────────────────────────────────┤
│    API Gateway / Backend            │
│  (Express/FastAPI/Django)           │
├─────────────────────────────────────┤
│    Business Logic Layer             │
│  (Controllers, Services)            │
├─────────────────────────────────────┤
│    Data Access Layer                │
│  (ORM: Sequelize/SQLAlchemy)        │
├─────────────────────────────────────┤
│    Database                         │
│  (PostgreSQL / MongoDB)             │
└─────────────────────────────────────┘
```

### Phase 2: State Management Evolution

```
Current (Phase 1):
  useState
  ↓
Phase 2 (Proposed):
  Context API + Custom Hooks
  OR
  Redux / Zustand
```

### Phase 3: Advanced Architecture

```
PWA Layer
  ├── Service Workers
  ├── Offline Support
  └── Push Notifications

Analytics Layer
  ├── Tracking
  ├── Reporting
  └── Insights

Integrations
  ├── Third-party APIs
  ├── Payment (future)
  └── Third-party services
```

---

## Performance Considerations

### Current Optimizations (MVP)

- ✅ CSS Modules (no style conflicts)
- ✅ Lazy loading preparation
- ✅ Efficient re-renders (proper dependency arrays)
- ✅ Optimized filtering/sorting

### Future Optimizations (Phase 2-3)

- [ ] React.memo for PantryItem list items
- [ ] useMemo for expensive calculations
- [ ] Code splitting with React.lazy()
- [ ] Virtual scrolling for large lists
- [ ] Pagination
- [ ] API caching

### Bundle Size

```
Target: < 200KB (gzipped)

Current estimate:
  React: ~40KB
  TypeScript libs: ~20KB
  date-fns: ~10KB
  App code: ~10KB
  ──────────────
  Total: ~80KB
```

---

## Security Considerations

### Current (MVP)

- ✅ Input validation in AddPantryForm
- ✅ Type safety with TypeScript
- ✅ XSS protection via React

### Phase 2 (Authentication)

- [ ] JWT token storage
- [ ] HTTPS only
- [ ] CSRF protection
- [ ] Rate limiting
- [ ] Input sanitization backend

### Phase 3 (Production)

- [ ] OAuth2 / OpenID Connect
- [ ] End-to-end encryption
- [ ] GDPR compliance
- [ ] Audit logging

---

## Deployment Architecture (Future)

### Development
```
git push → GitHub Actions → Run tests → Deploy to dev server
```

### Production (Phase 2+)
```
Frontend (Vercel/Netlify)
  ↓
API (Heroku/AWS/DigitalOcean)
  ↓
Database (AWS RDS/MongoDB Atlas)
```

---

## Conclusion

Architektura MVP jest **prosty, skalowalny i łatwy w utrzymaniu**. Każdy layer ma jasne odpowiedzialności, co ułatwia przyszłe rozszerzenia.

---

**Version**: 1.0  
**Last Updated**: Grudzień 2024
