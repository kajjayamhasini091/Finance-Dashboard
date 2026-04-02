# FinLedger — Finance Dashboard

A warm, editorial-styled finance dashboard. Single `index.html` file — no build tools, no dependencies to install. Open in any modern browser.

---

## Quick Start

```bash
# Option 1: Just open the file
open index.html

# Option 2: Serve locally (optional, for cleaner URLs)
npx serve .
# or
python3 -m http.server 3000
```

---

## Design Approach

The aesthetic is **editorial / financial magazine** — think The Economist or Financial Times. Warm cream backgrounds, ink-dark text, terracotta (`#c4552a`) as the primary accent. Typography pairs **Playfair Display** (serif headlines) with **Outfit** (clean body) and **IBM Plex Mono** for all financial figures.

Deliberately avoids the dark-glass or purple-gradient look common in dashboards. Feels like a physical ledger brought to life.

---

## Features

### Dashboard
- **4 Summary Cards** — Net Balance, Monthly Income, Expenses, Savings Rate; all with month-over-month % change
- **Balance Trend Chart** — Line chart with 7D / 30D / 3M period toggle
- **Spending Donut** — Category breakdown with inline legend and progress bars
- **Recent 5 Transactions** — Quick glance at latest activity

### Transactions
- Full filterable/sortable table
- Filter by **Type** (income/expense), **Category**, **Month**
- Sort by **Date** or **Amount** (toggle ascending/descending)
- **Global search** bar (description + category)
- **Export to CSV** — respects active filters

### Role-Based UI (Frontend Simulated)
| Role | Behavior |
|---|---|
| **Admin** | Sees Add, Edit, Delete buttons; clicking a row opens Edit modal |
| **Viewer** | All mutation UI hidden; read-only experience |

Switch roles via the dropdown in the sidebar.

### Insights
- Top spending category with lifetime total
- Average daily spending (by active expense days)
- Net cash flow for current month (green = surplus, red = deficit)
- 6-month grouped bar chart (income vs expenses)
- Category spending horizontal progress bars
- 12-week transaction activity heatmap

---

## State Management

All state lives in a single `S` object:

```js
const S = {
  txns: [],          // transaction array
  role: 'admin',     // 'admin' | 'viewer'
  filters: { type, cat, month, search },
  sort: { field, dir },
  editId: null,      // for edit modal
  period: '30d',     // chart period
  charts: {}         // Chart.js instances
}
```

**Persistence**: Transactions are saved to `localStorage` (`finledger_v2`) on every mutation. Seed data (28 transactions spanning Jan–Apr 2025) loads automatically on first visit.

---

## Tech Stack

| | |
|---|---|
| Framework | Vanilla JS — no framework, no build step |
| Charts | Chart.js 4.4 via CDN |
| Fonts | Playfair Display, Outfit, IBM Plex Mono (Google Fonts) |
| Styling | Pure CSS with custom properties |
| Data | Static seed + localStorage persistence |

---

## Project Structure

```
finledger/
├── index.html    ← Everything: markup, styles, and logic
└── README.md     ← This file
```

---

## Assumptions & Decisions

- Currency is Indian Rupees (₹); amounts formatted with `en-IN` locale
- "This month" is computed from the browser's system clock
- Investment transactions (e.g. SIP, Mutual Fund) count as expenses — money leaving the account
- The heatmap covers the last 84 days (12 weeks)
- Chart.js is loaded from cdnjs CDN; requires internet on first load

---

## Possible Extensions

- JWT-based auth + real backend RBAC
- Budget limits per category with alert badges
- Recurring transaction templates
- Multi-currency with live FX rates
- PWA + offline support
