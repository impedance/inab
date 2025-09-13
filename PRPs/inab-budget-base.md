name: "Inab Budget App Base PRP"
description: |
  Product requirement prompt for implementing a minimal budgeting application similar to YNAB using TypeScript and Next.js.

---

## Goal

**Feature Goal**: Deliver a minimal budgeting web application that tracks income and expenses, maintains transaction history, and supports budgeting by category.

**Deliverable**: Next.js 14 project with API routes, data models, and React components for accounts, categories, and transactions.

**Success Definition**: Users can create categories, record income/expense transactions, view categorized spending totals, and see remaining budget per category without errors.

## User Persona (if applicable)

**Target User**: Individual managing personal finances.

**Use Case**: Track day-to-day spending and categorize budget allocations.

**User Journey**:
1. Define budget categories and set planned amounts.
2. Record income or expense transactions against an account and category.
3. Review transaction history and remaining funds per category.

**Pain Points Addressed**:
- Difficulty tracking where money goes.
- Lack of visibility into category balances.

## Why

- Enables disciplined personal budgeting using YNAB principles.
- Establishes base for future enhancements like reports or syncing.
- Serves as minimal viable product for the inab budgeting tool.

## What

- SPA built with Next.js + React + TypeScript.
- Local JSON storage for budgets, categories, accounts, and transactions (no debts, no scheduled transactions).
- REST-like API routes for CRUD operations on categories, accounts, and transactions.
- UI components to display category budgets and transaction history.

### Success Criteria

- [ ] Categories can be created, updated, deleted, and budgeted amounts stored.
- [ ] Transactions record amount, date, account, category, and type (income/expense).
- [ ] Budget view shows remaining funds per category.
- [ ] All functionality implemented with strict TypeScript types.

## All Needed Context

### Context Completeness Check

_Before writing this PRP, validate: "If someone knew nothing about this codebase, would they have everything needed to implement this successfully?"_

### Documentation & References

```yaml
# MUST READ - Include these in your context window
- url: https://www.typescriptlang.org/docs/handbook/intro.html
  why: TypeScript syntax and typing fundamentals
  critical: Ensures correct interfaces and strict mode usage

- url: https://nextjs.org/docs/app/building-your-application/routing
  why: App Router conventions for API routes and pages
  critical: Follows Next.js file-based routing and server/client components

- url: https://react.dev/reference/react/useReducer
  why: State management pattern for transaction and category reducers
  critical: Avoids unnecessary external libraries for state

- docfile: PRPs/ai_docs/ynab_basics.md
  why: Captures YNAB budgeting principles to mimic functionality
  section: Core Entities
```

### Current Codebase tree (run `tree` in the root of the project) to get an overview of the codebase

```bash
root
├── .claude/
├── PRPs/
│   └── templates/
└── README.md
```

### Desired Codebase tree with files to be added and responsibility of file

```bash
root
├── app/
│   ├── page.tsx                    # Budget dashboard
│   └── api/
│       ├── categories/route.ts     # CRUD for categories
│       ├── accounts/route.ts       # CRUD for accounts
│       └── transactions/route.ts   # CRUD for transactions
├── lib/
│   └── types/
│       ├── budget.ts               # Budget, Category, Transaction interfaces
│       └── storage.ts              # JSON storage helper
├── components/
│   ├── CategoryList.tsx            # Display category budgets
│   ├── TransactionList.tsx         # Show transaction history
│   └── TransactionForm.tsx         # Form to add transactions
└── package.json
```

### Known Gotchas of our codebase & Library Quirks

```typescript
// CRITICAL: Next.js API routes must export named functions (GET, POST, DELETE)
// CRITICAL: 'use client' directive is required for interactive components
// CRITICAL: Maintain strict TypeScript settings to avoid implicit 'any'
// CRITICAL: JSON storage layer must handle file read/write concurrency safely
```

## Implementation Blueprint

### Data models and structure

```typescript
// lib/types/budget.ts
export interface Account {
  id: string;
  name: string;
  balance: number;
}

export interface Category {
  id: string;
  name: string;
  budgeted: number;
  activity: number;
  available: number;
}

export interface Transaction {
  id: string;
  accountId: string;
  categoryId: string;
  amount: number; // positive for income, negative for expense
  date: string;
  memo?: string;
}
```

### Implementation Tasks (ordered by dependencies)

```yaml
Task 1: CREATE Next.js project with TypeScript
  - IMPLEMENT: `npx create-next-app@latest inab --typescript`
  - NAMING: project folder `inab`
  - PLACEMENT: repository root

Task 2: CREATE lib/types/budget.ts
  - IMPLEMENT: TypeScript interfaces for Account, Category, Transaction
  - PATTERN: follow example in Implementation Blueprint
  - PLACEMENT: lib/types/

Task 3: CREATE lib/types/storage.ts
  - IMPLEMENT: Async JSON file read/write helpers using fs/promises
  - PATTERN: simple CRUD abstractions (load, save)
  - PLACEMENT: lib/types/

Task 4: CREATE app/api/categories/route.ts
  - IMPLEMENT: GET, POST, PUT, DELETE handlers
  - DEPENDENCIES: storage helpers and Category interface
  - PATTERN: Next.js App Router API route structure

Task 5: CREATE app/api/accounts/route.ts
  - IMPLEMENT: GET, POST, PUT, DELETE handlers
  - DEPENDENCIES: storage helpers and Account interface

Task 6: CREATE app/api/transactions/route.ts
  - IMPLEMENT: GET, POST, DELETE handlers (no update for simplicity)
  - DEPENDENCIES: storage helpers and Transaction interface

Task 7: CREATE components/CategoryList.tsx
  - IMPLEMENT: Displays budgeted, activity, and available for each category
  - DEPENDENCIES: fetch from categories API

Task 8: CREATE components/TransactionForm.tsx
  - IMPLEMENT: Form to add income or expense transaction
  - DEPENDENCIES: categories and accounts for selection

Task 9: CREATE components/TransactionList.tsx
  - IMPLEMENT: Shows transaction history grouped by date
  - DEPENDENCIES: transactions API

Task10: MODIFY app/page.tsx
  - IMPLEMENT: Client component aggregating CategoryList, TransactionList, TransactionForm
  - PATTERN: 'use client' at top, useReducer for local state
```

### Level 1: Type Safety & Code Quality Validation

```bash
npm run lint
npm run typecheck
npm run format
```

### Level 2: Unit Tests

```bash
npm test
```

### Level 3: Integration Testing (System Validation)

```bash
npm run build
npm run start &
sleep 5
curl -I http://localhost:3000 | head -n 1
curl http://localhost:3000/api/categories | jq '.length'
```

### Level 4: Creative & Domain-Specific Validation

```bash
npx tsc --noEmit --strict
npm run lint
```

## Final Validation Checklist

### Technical Validation

- [ ] All 4 validation levels completed successfully
- [ ] `npm test` passes with coverage
- [ ] `npm run lint` yields no errors
- [ ] `npx tsc --noEmit` passes
- [ ] Production build `npm run build` succeeds

### Feature Validation

- [ ] Budget totals update correctly after transactions
- [ ] Category balances never drop below zero without explicit adjustment
- [ ] User can add and remove categories, accounts, transactions

### Code Quality Validation

- [ ] Follows TypeScript strict mode
- [ ] React components use functional patterns and hooks
- [ ] No unused variables or implicit any types

### TypeScript/Next.js Specific

- [ ] API route functions exported as named handlers
- [ ] Client components include `'use client'` directive

### Documentation & Deployment

- [ ] README updated with setup instructions
- [ ] Environment variables documented if later introduced

---

## Anti-Patterns to Avoid

- ❌ Skipping type annotations for speed
- ❌ Mutating state directly in reducers
- ❌ Adding external dependencies for trivial tasks
- ❌ Mixing server and client logic without boundaries

---

## Confidence Rating

8/10 – The PRP includes core patterns, references, and validation gates. Some assumptions about Next.js 14 and JSON storage may require adjustment when integrating with real infrastructure.
