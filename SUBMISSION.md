# Module 3 submission

Project: **Feedback Board**. A visitor enters a name and message, the app saves it to a database, and the recent-feedback list displays it after refresh.

- Source: https://github.com/Gauravwakle96/module-3
- Local preview: http://localhost:8013
- Live website: https://module3-feedback-board.onrender.com
- LinkedIn caption and live screenshots: [linkedin/](linkedin/).

Cloud deployment verified on 9 November 2026: Render reports a live deployment on the Free plan, `/health` reports PostgreSQL, and feedback submitted through the HTTPS page remains visible after reload. The database is in a separate Supabase Free project with RLS enabled and a TLS session-pooler connection.

## Requirement mapping

| Module 3 topic | Project implementation / exercise |
| --- | --- |
| Cloud compute | Deploy the Python app as a Render Free web service |
| Managed database | Supabase Free PostgreSQL stores feedback independently of the app's temporary disk |
| Networking | Render routes the public HTTPS URL to Uvicorn's `$PORT`; the backend connects to Supabase's IPv4 session pooler over TLS |
| IAM and permissions | Cloud account access controls deployment; Supabase RLS blocks direct anonymous Data API access; the backend keeps its database credential private |
| Environment variables and secrets | `DATABASE_URL` lives only in server environment settings; `.env` and local DB files are ignored |
| Logs and monitoring | `/health` performs a database readiness check; sanitized request logs appear in Render Logs; provider dashboards show resource usage |
| Quotas | 500 saved messages and 10 submissions/minute by default; stay on both providers' Free plans |
| Object storage exploration | Supabase includes object storage; the app has no uploads and needs only bundled static files, so no storage bucket is created |
| Deployment flow | GitHub source → Render compute → Supabase database → HTTPS networking → live page |

Compute, managed database, networking, IAM, quotas, and logging are exercised by the live deployment. Object storage is explored as a service but is not implemented. This minimal project has no VPC configuration, cloud object uploads, or custom monitoring service.

## Finish the submission

1. Submit the GitHub repository URL and live website URL above.
2. Upload `linkedin/03-live-desktop.png` and `linkedin/04-live-mobile.png` using `linkedin/post.txt` as the caption.
3. Submit the resulting LinkedIn post link. The caption and screenshots are prepared; posting to LinkedIn is the remaining user step.

`linkedin/live-checks.json` records the public HTTPS and PostgreSQL checks. The original local screenshots and SQLite tests are retained as development evidence. Free services can sleep or pause after inactivity; open the page before submitting and allow Render time to wake.
