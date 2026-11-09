# Verification

Checked on 9 November 2026 using the locked environment.

- Ruff lint and format: passed.
- Mypy: passed for the four application modules.
- Pytest: **25 passed**. Covers persistence across app restarts, input validation, latest-ten ordering, atomic concurrent quota enforcement, rate limits, generic database errors, host/origin checks, SQL text safety, and redacted logging.
- Build: source distribution and Python wheel built successfully, including bundled HTML/CSS/JavaScript.
- Dependency audit: **no known vulnerabilities found** in the module's installed dependency set. The local project is not listed on PyPI and was skipped by the advisory scanner.
- Browser: submit, refresh, persistence on reload, keyboard controls, error recovery, and plain-text HTML rendering passed in headless Chrome. No JavaScript errors.
- Responsive/accessibility: no horizontal overflow and no axe WCAG A/AA violations at 1280, 768, and 390 px widths, with reduced-motion preference enabled.

One third-party test warning: Starlette's TestClient reports that its HTTPX integration is deprecated in favor of HTTPX2. It did not affect the passing results.

Cloud verification completed on 9 November 2026:

- Supabase project `nivrxkypsrzvjvpolqvr` is in a Free organization. The schema was applied, RLS is enabled, and the session-pooler client connection uses TLS.
- The application's PostgreSQL add/list flow passed against the managed database.
- Render service `srv-dbou2jp5efls73f0ssug` uses the Free plan. Build and deployment succeeded at https://module3-feedback-board.onrender.com.
- Public `/health` returned `{"status":"ok","database":"postgresql"}`.
- The real HTTPS browser flow submitted feedback, reloaded the page, and verified persistence through the API. No JavaScript errors or axe WCAG A/AA violations at desktop, tablet, and mobile widths.
- Actual live screenshots and the public verification report are in `linkedin/03-live-desktop.png`, `linkedin/04-live-mobile.png`, and `linkedin/live-checks.json`.

Provider CLI credentials and the database password are held outside the repository. Auto-deploy was disabled to avoid unnecessary rebuilds for documentation changes; future code deployments can be triggered manually from Render.
