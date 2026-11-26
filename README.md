# PROJECT YT — Railway License Panel

## Included
- Flask admin login and dashboard
- License generation: 1 / 7 / 30 / 365 days, lifetime, custom date
- Single, multi-device and unlimited-device license limits
- Enable/disable/delete/reset device bindings
- TXT/CSV export
- Online update and maintenance controls
- License verification, config, update and admin JSON APIs
- Firebase-free in-app announcements using HTTP polling

## Railway environment variables
Set these in Railway → Service → Variables:

- `KURO_SECRET_KEY` = long random secret (keep private)
- `KURO_ADMIN_USERNAME` = admin username (at least 3 chars)
- `KURO_ADMIN_PASSWORD` = strong password (at least 12 chars)
- `KURO_DB_PATH` = `/data/project_yt.db`

Attach a Railway Volume mounted at `/data` to persist the SQLite database.

## Deploy
Push the contents of this folder to your GitHub repository connected to Railway.
Railway uses `Procfile` to start Gunicorn. After deployment open `/login`.

If the admin database was already initialized before setting username/password,
the first admin will not be recreated automatically. For a fresh install, use a
new/empty database volume. Do not delete an existing production database unless
you have backed it up.

## API
- `POST /api/v1/verify` JSON: `{"key":"PROJECT-YT-...","device_id":"device-123"}`
- `GET /api/v1/config`
- `GET /api/v1/update?version=1.0.0`
- `GET /api/v1/notifications?after=0`
- `GET /health`

## Firebase-free notification limitation
The admin notification page stores announcements on your server. Android app
must poll `/api/v1/notifications?after=LAST_ID` while running and display new
messages. This is not guaranteed OS push: after the app is closed or force-stopped,
Android will not reliably run polling in the background. True system push delivery
requires a push transport/provider and Android-side integration.

## Fix for the reported Railway error
This package has an `api_docs` Flask endpoint and the `base.html` navigation
links to it. Deploy the complete package together; do not mix old templates
with a different `app.py`.
