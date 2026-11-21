# MailFlow on Railway

[MailFlow](https://github.com/maathimself/mailflow) (AGPL-3.0) — a self-hosted unified webmail for many IMAP accounts, with push notifications, rules, contacts and search. Deploy it on Railway with one click.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/TEMPLATE_CODE)

## What this template runs

- **frontend**: `ghcr.io/onepush-dev/mailflow-frontend:4.5.1`, the official `mailflow-frontend:4.5.1` image (Caddy + React app, code unmodified) with `TLS_MODE=off`, so it serves HTTP on port 80 behind Railway's HTTPS edge and proxies `/api` to the backend over the private network. Public domain.
- **backend**: `ghcr.io/onepush-dev/mailflow-backend:4.5.1`, the official `mailflow-backend:4.5.1` image (Node, code unmodified) on port 8080 (Railway's default PORT), private only. Migrations run on boot. Healthcheck `/api/health`.
- **postgres**: `postgres:16-alpine` with a volume at `/var/lib/postgresql`, private only.

## Variables

| Service | Variable | Value | Notes |
|---|---|---|---|
| frontend | `BACKEND_HOST` | `${{backend.RAILWAY_PRIVATE_DOMAIN}}` | Where Caddy proxies the API. |
| backend | `APP_URL` | `https://${{frontend.RAILWAY_PUBLIC_DOMAIN}}` | Public URL (CORS, WebSocket origin, links). |
| backend | `FRONTEND_URL` | `https://${{frontend.RAILWAY_PUBLIC_DOMAIN}}` | Same as `APP_URL`. |
| backend | `SESSION_SECRET` | generated secret (64 chars) | Signs session cookies. |
| backend | `ENCRYPTION_KEY` | generated 64-hex secret | Encrypts stored IMAP/SMTP passwords and OAuth tokens. Never change it after first use. |
| backend | `DB_HOST` | `${{postgres.RAILWAY_PRIVATE_DOMAIN}}` | Postgres host. |
| backend | `DB_PASSWORD` | `${{postgres.POSTGRES_PASSWORD}}` | Postgres password. |
| postgres | `POSTGRES_PASSWORD` | generated secret | Postgres superuser password. |

Baked into the images: `TLS_MODE=off`, `BACKEND_PORT=8080` (frontend); `NODE_ENV=production`, `PORT=8080`, `TRUST_PROXY_HOPS=2` (Railway edge + Caddy), `DB_PORT=5432`, `DB_NAME=postgres`, `DB_USER=postgres` (backend). Optional extras you can add on the backend: `VAPID_PUBLIC_KEY` / `VAPID_PRIVATE_KEY` / `VAPID_SUBJECT` (web push), `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET`, `MS_CLIENT_ID` / `MS_CLIENT_SECRET` (OAuth mail accounts), `SESSION_IDLE_DAYS`.

## How to use

1. Deploy and open the frontend domain.
2. The first account you register becomes the admin; registration then closes unless you invite users.
3. Add your IMAP/SMTP accounts (Gmail, Outlook, Fastmail, any IMAP server) under Settings.

Not affiliated with MailFlow. MailFlow is AGPL-3.0 (with a separate commercial licence from its author); this repo only adds Railway defaults on top of the official images.
