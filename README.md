# Module 3: Feedback Board

A simple one-page project for **Cloud Services & Web App Deployment**. Enter a name and message, save it, and see the ten latest messages. Uses Python/FastAPI, plain HTML/CSS/JavaScript, and PostgreSQL on Supabase. No frontend build, paid API, login, or Docker is needed.

Repository: https://github.com/Gauravwakle96/module-3

## Local preview

Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and Python 3.11 or newer.

```powershell
git clone https://github.com/Gauravwakle96/module-3.git
cd module-3
uv sync --locked
uv run uvicorn feedback_board.main:app --host 127.0.0.1 --port 8013 --no-access-log
```

Open http://localhost:8013. Without `DATABASE_URL`, feedback is stored in the ignored local `feedback.sqlite3` file. This is a local preview; a live Module 3 submission uses the cloud deployment below.

This repository contains only Module 3. Run the app commands from the repository root. An ordinary virtual environment also works: install `requirements.txt` with pip, then run the same uvicorn command without `uv run`.

## Free cloud deployment

1. Sign in to [Supabase](https://supabase.com/dashboard). Create a **Free** project and keep its database password private.
2. Open its SQL Editor and run [schema.sql](schema.sql). Open **Connect → Session pooler** and copy the PostgreSQL URL. Replace the password placeholder, percent-encoding reserved characters in the password. The session pooler supports IPv4 without buying the IPv4 add-on.
3. Sign in to [Render](https://dashboard.render.com). Choose **New → Web Service**, connect the GitHub repository, and use these settings:

   | Setting | Value |
   | --- | --- |
   | Repository | `Gauravwakle96/module-3` |
   | Branch | `main` |
   | Root directory | Leave blank (repository root) |
   | Runtime | Python 3 |
   | Build command | `pip install -r requirements.txt` |
   | Start command | `uvicorn feedback_board.main:app --host 0.0.0.0 --port $PORT --workers 1 --no-access-log` |
   | Instance plan | **Free** |
   | Health check | `/health` |

4. Add server environment variables `ENVIRONMENT=cloud` and `DATABASE_URL=<your private session-pooler URL>`. Leave the URL out of GitHub, screenshots, and browser code. Render's automatic `RENDER_EXTERNAL_HOSTNAME` configures the accepted hostname.
5. Deploy. Open the HTTPS URL assigned by Render, submit one message, and refresh to confirm it persists. `/health` should return `{"status":"ok","database":"postgresql"}`. Keep the URL in your submission and LinkedIn post.

Alternatively create a Render **Blueprint** using the file path `render.yaml`; supply `DATABASE_URL` when prompted. Never switch to a paid instance to complete this exercise.

Render Free sleeps after 15 idle minutes, may take about a minute to wake, and shares 750 free instance hours per workspace each month. Its local disk is temporary, which is why the live app requires PostgreSQL. Supabase Free includes a 500 MB database and pauses projects after a week of inactivity. Check the dashboards before submitting; free tiers suit a class demo. No payment method is needed for the app's selected plans.

## Configuration and behavior

Copy `.env.example` to `.env` for optional local configuration. The server reads environment variables and the local `.env`; cloud secrets go in Render's environment settings.

| Variable | Default | Purpose |
| --- | --- | --- |
| `ENVIRONMENT` | `local` | `cloud` requires PostgreSQL; prevents accidental SQLite deployment |
| `DATABASE_URL` | empty | Private PostgreSQL session-pooler URL; connections require TLS |
| `LOCAL_DB_PATH` | `feedback.sqlite3` | Local preview database file |
| `ALLOWED_HOSTS` | `localhost,127.0.0.1` | Additional permitted hostnames, comma separated |
| `FEEDBACK_LIMIT` | `500` | Maximum saved messages, 1–5000 |
| `WRITE_LIMIT` | `10` | Total submissions per minute per process, 1–100 |

`GET /api/feedback` returns `{total, items}`. `POST /api/feedback` accepts JSON `{name, message}` and returns the saved message with HTTP 201. Names are 1–60 characters and messages 1–500 after trimming. Errors use 422 for invalid input, 413 for bodies over 8 KiB, 429 for rate limits, 409 for a full board, and 503 for a database failure. `/health` checks the database. No browser CORS access is enabled.

The app uses parameterized queries, database constraints, atomic quota checks, plain-text rendering, a Content Security Policy, and host/origin checks. Feedback is public; do not enter private information. SQL enables RLS and removes direct anonymous API access. The server uses the project owner's database credential for this small demo; it must remain private. There is no moderation, login, or deletion UI.

The one-worker write limit is process-local and resets on restart. It is sufficient for a small submission demo, not a distributed abuse-control system. Request logs contain fixed route names, statuses, and timing; they omit submitted names/messages, bodies, and database credentials. View them in Render's **Logs** and check resource usage in each provider's dashboard.

## Checks

```powershell
uv run ruff check .
uv run ruff format --check .
uv run mypy
uv run pytest
uv build
uvx pip-audit --path .venv/Lib/site-packages
```

See [SUBMISSION.md](SUBMISSION.md) for the Module 3 requirement mapping and [linkedin/post.txt](linkedin/post.txt) for a caption. Screenshots in that folder are local previews until replaced with captures from the live URL.

## Primary references and licenses

- [Render FastAPI deployment](https://render.com/docs/deploy-fastapi), [Free plan](https://render.com/docs/free), [Blueprint configuration](https://render.com/docs/blueprint-spec).
- [Supabase database connections](https://supabase.com/docs/guides/database/connecting-to-postgres), [RLS](https://supabase.com/docs/guides/database/postgres/row-level-security), [Free plan](https://supabase.com/pricing).
- [Psycopg usage](https://www.psycopg.org/psycopg3/docs/basic/usage.html), [OWASP cheat sheets](https://cheatsheetseries.owasp.org/).
- Project code uses the repository's MIT license. FastAPI, Pydantic, and pydantic-settings use MIT; Uvicorn uses BSD-3-Clause; Psycopg uses LGPL-3.0. Dependencies keep their own licenses. The UI uses system fonts and no external images.
