# Module 3: Simple Feedback Board

## Scope

A one-page web app for the Module 3 submission. Visitors enter a name and a short message; the app saves feedback to a database and displays the ten latest messages. Keep this project in its own Module 3 repository.

## Stack and deployment

- FastAPI serves the page and a small JSON API.
- Plain HTML, CSS, and JavaScript; system fonts and no frontend framework/build.
- PostgreSQL on Supabase's free tier for cloud persistence, using psycopg and parameterized SQL.
- SQLite only for a local preview without a cloud account. It is not the production database.
- Render's free Python web service, automatic HTTPS, and a single process.
- Environment variables hold the database connection string and resource limits. No model API, paid plan, Docker, or CI/CD feature is needed for this module.

## Data and API

One table: `module3_feedback` with id, name (1–60 characters), message (1–500 characters), and UTC creation time. `schema.sql` is the initial cloud schema. Browser clients never receive the database URL.

- `GET /`: feedback page.
- `GET /health`: application/database readiness, no connection details.
- `GET /api/feedback`: ten latest messages and the total count.
- `POST /api/feedback`: validate, rate limit, persist, return the saved message.
- `POST` is public by design; there are no admin/delete actions or accounts.
- A 500-message default quota prevents unbounded storage. Database failures return a retriable error without secrets.

## Security and UI

Use parameterized SQL, explicit input bounds, plain text rendering, strict CSP, host checks, and process-local write limits. Enable PostgreSQL RLS so the table is not exposed through Supabase's anonymous API. Render stores the database URL as a server secret. Log request method, fixed route, result, and timing; omit names, messages, URLs, keys, and request bodies.

Simple centered page: title, short instruction, two labeled fields, submit button, recent feedback. No decorative assets, custom fonts, theme switcher, charts, or dashboards. Cover loading, empty, errors, and successful submission. Check keyboard controls and mobile layout.

## Completion

1. Confirm official free-tier/deployment/database instructions.
2. Implement the page, API, local preview, and PostgreSQL schema.
3. Run focused persistence, validation, error, quota, and security tests; inspect desktop/mobile rendering.
4. Add Render configuration, concise deployment steps, Module 3 mapping, and a LinkedIn caption.
5. Push to `Gauravwakle96/module-3` and provide the local preview/submission ZIP.
6. Deploy and verify the live service when the user's free cloud accounts are accessible. Until then, label cloud deployment as pending; do not claim a live URL.
