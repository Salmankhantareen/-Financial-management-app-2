# Hilal Amin Construction Company — Financial Management System

This is a modular HTML5/CSS3/vanilla JavaScript prototype for a construction-company financial management workspace. It uses IndexedDB for structured records and LocalStorage/sessionStorage for preferences and development sessions.

## Run locally

From this directory, start a static server (ES modules require HTTP):

```bash
python3 -m http.server 8000 --bind 0.0.0.0
```

Open `http://localhost:8000/`.

## Demo accounts

- **Super Administrator** — `admin@hilalamin.com` / `Admin@123`
- **Finance Manager** — `finance@hilalamin.com` / `Finance@123`
- **Project User** — `user@hilalamin.com` / `User@123`

## Notes

- Demo records are labelled in the interface and can be reset from System Settings → Data management.
- The supplied PDF references were not present in the workspace at build time. The visual language follows the navy, blue, glass-card brief, and the Chart of Accounts is a structured 236-row demonstration import with explicit `demo`/`sourceNote` metadata so it can be replaced with the source rows when supplied.
- Local browser storage is device/browser scoped, not a centralized multi-user database. Replace `js/database.js` with a REST/API adapter and move authentication, authorization, validation, transactions, backup and audit logging to a server before production.

See `BACKEND_INTEGRATION.md` for the REST/database/security migration boundary.

## Test workflow

1. Sign in as the administrator and open Dashboard.
2. Open Transaction Vouchers → New voucher. Add balanced lines, save draft, submit, approve and post.
3. Confirm the dashboard, ledger and account balances update from posted entries.
4. Use Projects, Cost Centers and Budgets to create/edit records.
5. Use Reports to print or export CSV.
6. Use System Settings → Data management to export a backup, reset demo data, or restore a JSON backup.
7. Sign in as Project User to verify assigned navigation and submit-only workflow.
