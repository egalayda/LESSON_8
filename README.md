# My Dashboard

A single-page analytics dashboard built with Vue 3, Vuetify 3, and Chart.js. Displays monthly business metrics — revenue, visitors, conversions, and orders — with interactive filtering and responsive data visualizations.

**Live demo:** [lesson-8-phi.vercel.app](https://lesson-8-phi.vercel.app)

## Screenshot

<!-- Replace with an actual screenshot of the dashboard -->
![Dashboard Screenshot](screenshot-placeholder.png)

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Vue 3 + TypeScript |
| Build Tool | Vite |
| UI Library | Vuetify 3 (dark theme) |
| Charts | Chart.js via vue-chartjs |
| Icons | Material Design Icons (`@mdi/font`) |
| Hosting | Vercel |

## Features

- **Month picker** — filter all cards and charts by a single month, or view the full year with "ALL"
- **Summary cards** — revenue, visitors, conversions, and orders with up/down change indicators
- **Bar chart** — monthly revenue breakdown
- **Line chart** — visitor trends over time
- **Area chart** — conversion rate trend (full-width)
- **Dark theme** — Vuetify dark mode enabled by default
- **Responsive layout** — cards and charts stack on mobile

## Project Structure

```
my-dashboard/
├── src/
│   ├── App.vue            # Single-page dashboard (layout, cards, charts, filtering)
│   ├── main.ts            # Entry point — Vuetify + MDI setup
│   └── data/
│       └── metrics.json   # 12 months of synthetic business data
├── public/
│   └── favicon.ico
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
└── vercel.json            # SPA rewrite rules for Vercel
```

## Setup

**Prerequisites:** Node.js 20.19+ or 22.12+

```bash
# Clone the repo
git clone https://github.com/egalayda/LESSON_8.git
cd LESSON_8/my-dashboard

# Install dependencies
npm install

# Start the dev server
npm run dev
```

Open http://localhost:5173 in your browser.

### Build for Production

```bash
npm run build
```

Output is written to `dist/`.

### Preview the Production Build

```bash
npm run preview
```

## Data

The dashboard reads from `src/data/metrics.json` — a local JSON file containing 12 months (Jan–Dec 2025) of synthetic business data:

| Field | Type | Description |
|---|---|---|
| `revenue` | number | Dollar amount ($41K–$79K), trending upward |
| `visitors` | number | Site visitors with seasonal pattern (higher in summer) |
| `conversions` | number | Percentage (2.3%–4.5%), natural fluctuation |
| `orders` | number | Order count, loosely correlated with visitors |

No external APIs are called. To use real data, replace `metrics.json` with the same schema.

## Deployment

The project is deployed to **Vercel** via GitHub integration. On every push to `main`, Vercel automatically:

1. Runs `cd my-dashboard && npm install && npm run build`
2. Serves the `my-dashboard/dist` output
3. Applies SPA rewrites from `vercel.json`

No additional configuration is required beyond the initial Vercel project setup with **Root Directory** set to `my-dashboard`.