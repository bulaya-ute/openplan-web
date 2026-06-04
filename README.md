# OpenPlan Web

The React web client for [OpenPlan](https://github.com/bulaya-ute/openplan-api) — a self-hostable, open-source task manager.

**License:** MIT · **Status:** Working prototype

---

## Tech Stack

- React 19 + TypeScript
- Vite 8 + Tailwind CSS v4
- Zustand 5 (state management)
- Axios (HTTP + JWT interceptor)
- React Router v7
- date-fns, lucide-react

## Quick Start

### Prerequisites

- Node.js 20+
- A running [OpenPlan API](https://github.com/bulaya-ute/openplan-api)

### Run

```bash
npm install
npm run dev      # http://localhost:5041
```

To point at a non-default API URL, create a `.env` file:

```env
VITE_API_URL=http://localhost:5040/api/v1
```

## Commands

```bash
npm run dev      # Vite dev server
npm run build    # tsc -b && vite build → dist/
npm run lint     # ESLint
npm run preview  # Preview production build
```

## Documentation

| Document | Description |
|---|---|
| [Architecture](docs/architecture.md) | State management, routing, auth, theme |
| [Setup](docs/setup.md) | Local dev and production deployment |

## Versioning

The current version is recorded in [`version.json`](version.json) at the repo root. This file is read by the admin panel and must match the corresponding GitHub release tag exactly.

## License

[MIT](LICENSE)
