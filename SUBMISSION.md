# Module 3 submission

Project: **Feedback Board**. A visitor enters a name and message, the app saves it to a database, and the recent-feedback list displays it after refresh.

- Source: https://github.com/Gauravwakle96/module-3
- Local preview: http://localhost:8013
- Live website: **pending free Render/Supabase account setup and deployment**. Replace this line with the actual verified Render HTTPS URL before submission.
- LinkedIn caption and local screenshots: [linkedin/](linkedin/).

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

Compute, managed database, networking, IAM, quotas, and logging are exercised when you complete the deployment steps in [README.md](README.md). Object storage is explored as a service but is not implemented. This minimal project has no VPC configuration, cloud object uploads, or custom monitoring service.

## Finish the submission

1. Follow the free deployment instructions in `README.md`.
2. Open the actual HTTPS page, submit a message, refresh, and confirm `/health` reports `postgresql`.
3. Take a screenshot of the live page and Render's successful deployment. Do not include database URLs/passwords or environment-variable values.
4. Add the verified URL here and in the LinkedIn caption, then submit the GitHub URL, live URL, and post link.

Local screenshots and SQLite tests demonstrate the application flow; they do not prove a cloud deployment. The supplied LinkedIn caption accurately describes the current local result and deployment preparation.
