# Setup & Deployment

## Local Development

### Prerequisites

- Node.js 20+
- A running [OpenPlan API](https://github.com/bulaya-ute/openplan-api)

### Run

```bash
npm install
npm run dev      # http://localhost:5173
```

Create `.env` to override the API URL:

```env
VITE_API_URL=http://localhost:5000/api/v1
```

---

## Production Build

```bash
npm run build    # outputs to dist/
```

The `dist/` folder contains a fully static SPA. Serve it from any static file host (Nginx, Caddy, S3, Vercel, etc.).

### Nginx Example

```nginx
server {
    listen 443 ssl;
    server_name app.yourdomain.com;

    root /var/www/openplan-web;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Copy `dist/` contents to `/var/www/openplan-web`.

### Environment at Build Time

The `VITE_API_URL` variable is baked in at build time. Set it before running `npm run build`:

```bash
VITE_API_URL=https://api.yourdomain.com/api/v1 npm run build
```

---

## Version Switching

The admin panel can trigger a version switch for the web app. The updater daemon on the server pulls the target release, runs `npm ci && npm run build`, and replaces the `dist/` folder. No server restart is needed for static file updates.

The current version is tracked in [`version.json`](../version.json).
