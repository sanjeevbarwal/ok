# DSR Pro — Render deployment-ready **STAGING** package

This package configures the existing Python DSR pilot for Render Blueprint deployment. **It is not live, not a production-ready app, and not suitable for real employee/customer records.** Native Android/iOS apps are not included; the site works in a mobile browser.

## Simplified deployment (Render + GitHub)

1. Obtain company IT approval for **synthetic-data-only staging** and review Render costs. The Blueprint requests a **paid Starter web service** and a **persistent 1 GB disk**; free ephemeral storage would erase SQLite records on restarts/deployments. Verify pricing and region with Render before approval.
2. Extract the ZIP. Create a **private GitHub repository**, upload the **contents** of this folder (including `render.yaml` and `Dockerfile`), and keep all secrets out of Git.
3. In Render, choose **New → Blueprint**, connect your GitHub repository, and select the repository containing `render.yaml`. Review the proposed resources and charges.
4. Set `DSR_ADMIN_PASSWORD` to a **unique random password of at least 24 characters** when prompted. Do not publish or share it. Deploy the Blueprint.
5. Render will supply a temporary `*.onrender.com` URL and manage HTTPS. On the first run, sign in with employee code `ADMIN001` and the password you configured. **Change the default admin access strategy before any real use.**
6. Create **fictional** users, test role restrictions, DSR entry, No Activity, correction requests, exports, and mobile layouts. Confirm disk persistence after redeploy and establish database backups.

## What is included
- Existing DSR web pilot (`app/app.py`, `app/fields.json`)
- `Dockerfile` and `render.yaml` for one-Blueprint provisioning
- Persistent SQLite disk mount and HTTPS via Render
- README and release-blocker checklist

## Important limitations / release blockers
- The inherited pilot lacks CSRF protection, login rate limits, robust persistent sessions, SSO/MFA, password recovery, complete validation, penetration testing, and production-grade audit/security controls.
- Reporting history is not permissioned by historical manager assignments.
- Approved corrections are reviewed but not applied to the original DSR.
- Automated reminders, approved-leave workflow, and 8 PM deadline enforcement are not implemented.
- Native mobile applications are not built; this is mobile-browser access.
- SQLite on a single persistent disk is for staging only; use managed PostgreSQL and a reviewed migration plan for production.
- The app is publicly reachable by URL after deployment. Do **not** use real personal/customer data, even with HTTPS. Restrict access using a reviewed staging access gateway/network policy before inviting testers.
- Company security, privacy, data residency, consent, retention, and IT approvals are required for real-data deployment.

## Technical notes
- `DSR_DB=/data/dsr.sqlite3` is persisted on the Render disk; **do not use the free ephemeral instance** for persistent testing.
- The application listens on port 8080; Render terminates TLS. The secure-cookie flag is enabled with `DSR_HTTPS=1`.
- Render's Blueprint configuration does not itself provision scheduled backups. Arrange a consistent SQLite backup and test recovery.
- Do not commit `.env`, passwords, customer exports, or local databases.
- Service name and region are suggestions and can be changed before deployment.
