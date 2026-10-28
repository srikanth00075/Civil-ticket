# Civic Ticket

A ward-level civic complaint management system. Citizens file geotagged
complaints with photos; the public board tracks them through Open → In
Progress → Resolved, and unresolved reports automatically escalate through
category-specific SLA tiers.

## Features

- **Citizen reporting**: title, category, description, photo upload, map-pinned location
- **Duplicate detection**: warns about a nearby open report of the same category and offers to upvote it instead
- **Public board**: list/map views, search, filters, sort (recent / most upvoted / longest waiting), upvotes and comments
- **SLA escalation**: per-category timelines escalate tickets to review → ward officer → commissioner
- **Resolution loop**: before/after proof photos and citizen satisfaction ratings
- **Admin panel** (passcode-gated): priority queue, officer assignment, status updates, CSV export
- **Analytics ledger**: resolution rate, average resolution time, category/status breakdowns, 14-day filing trend
- **Live sync banner**: polls storage and prompts a refresh when other users change the board

## Getting started

```bash
npm install
npm run dev
```

Then open the URL Vite prints (usually http://localhost:5173).

Demo admin passcode: `WARD14`.

## Architecture

Client-only React single-page app in three layers:

| Layer | Responsibility |
|---|---|
| Presentation | `TicketStub`, `TownMap`, `Ledger`, forms and admin UI |
| Logic & state | React state/hooks: SLA escalation, priority scoring, filtering, duplicate detection, polling |
| Storage | `window.storage` key-value API (shared registry + personal store) |

See [`docs/CivicTicket_Project_Report.docx`](docs/CivicTicket_Project_Report.docx) for the abstract, architecture drawings, data dictionary and design patterns.

## Important limitations

- **Storage is local.** `src/storage.js` emulates `window.storage` with `localStorage`, so data lives in one browser and is not shared across users. For a real multi-user deployment, swap in a backend adapter with the same `get/set/delete/list` interface.
- **The admin passcode is client-side only.** It is visible in the JavaScript bundle and is a UX gate, not real authentication. Production use needs server-side auth.

## Project structure

```
civic-ticket/
├── index.html
├── package.json
├── vite.config.js
├── docs/
│   └── CivicTicket_Project_Report.docx
└── src/
    ├── main.jsx
    ├── storage.js
    └── CivicComplaintPortal.jsx
```
