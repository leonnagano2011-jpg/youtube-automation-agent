# Base44 dev environment notes

- Single Node/Express service (`index.js`) serving the dashboard + API on port 3456; mapped to host port 3000.
- Run with: `docker compose -f docker-compose.base44.yml up -d` (npm install happens at container start; sqlite3/sharp/playwright deps compile there, first boot takes ~1-2 min).
- SQLite data lives in `./data` (created on boot); uploads in `./uploads`.
- Without an AI provider key and YouTube OAuth, the app boots in "setup mode": dashboard works, generation/publishing disabled. Add keys via platform secrets (`/run/base44/app.env`); non-secret defaults live in `.env.base44-defaults`.
- YouTube authorization requires the interactive `npm run walkthrough` / `npm run credentials:setup` inside the container.
- Health check: `curl localhost:3000/health`.
