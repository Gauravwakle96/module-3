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

The local verification used SQLite. Supabase connectivity, PostgreSQL execution, TLS networking, and the live Render deployment have **not** been verified because cloud account setup is pending. Follow `README.md` before submitting a live URL.
