# AgentTube (YouTube Automation Agent) — Base44 Dev Notes

## Overview
Node.js/Express app ("AgentTube" / "Lumen") that automates a YouTube channel end to end.
Serves a dashboard (static HTML/JS/CSS in `dashboard/`) on port 3000, with a REST API.
Uses SQLite (`data/youtube_automation.db`, auto-created) for persistence.

## Running in Base44
- `docker compose -f docker-compose.base44.yml up -d` — starts the app on port 3000.
- Base image: `node:22` (includes build tools for native modules like `sqlite3` and `sharp`).
- Source is bind-mounted at `/app`; `node_modules` lives in a named volume to avoid host conflicts.
- Start command: `npm install && npx nodemon index.js` — live reload on `.js`/`.json` changes.
- Healthcheck: `GET /health` returns JSON with `status` and `setupRequired`.

## Boot behavior
- The app boots in **setup mode** when AI provider credentials are missing — the dashboard
  is fully visible but content generation and publishing are disabled.
- No external credentials are required to boot. All AI provider keys (OpenAI, Gemini,
  OpenRouter, etc.) are optional and configured via `npm run walkthrough` or `.env`.
- To enable full functionality, the user needs at least one AI provider key and YouTube
  OAuth credentials (client ID/secret). These are external secrets the user must supply.

## Key files
- `index.js` — main app, Express server, all API routes.
- `dashboard/` — static frontend (index.html, app.js, styles.css, enhance.js).
- `database/db.js` — SQLite initialization and schema.
- `utils/credential-manager.js` — validates credentials; returns false if missing.
- `agents/` — content strategy, script writer, thumbnail, SEO, production, publishing, analytics agents.
- `schedules/daily-automation.js` — cron-based automation scheduler.

## Verification
- `curl http://localhost:3000/health` → `{"status":"setup_required","initialized":true,...}`
- `curl http://localhost:3000/` → dashboard HTML page.
- Container status should be `Up (healthy)`.
