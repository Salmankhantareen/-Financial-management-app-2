# Backend integration boundary

The browser prototype intentionally keeps persistence behind `js/database.js` and accounting/authorization behind `js/accounting.js` and `js/auth.js`. Before production, replace those adapters; do not promote LocalStorage or IndexedDB into a multi-user security model.

## Recommended services

- `POST /api/auth/login` — server-side credential verification, MFA/session token and account status.
- `POST /api/auth/logout` and `POST /api/auth/change-password`.
- `GET/POST/PATCH /api/users` — server-enforced role and permission assignments.
- `GET/POST/PATCH /api/projects`, `/cost-centers`, `/employees`, `/suppliers`, `/customers`.
- `GET/POST /api/transactions` — create draft and validate relationships on the server.
- `POST /api/transactions/:id/submit`, `/approve`, `/reject`, `/post`, `/reverse`.
- `GET /api/reports/*` — calculate reports from posted journal lines in database transactions.
- `GET/POST /api/exchange-rates` — effective-dated rates; never mutate historical transaction valuations.
- `GET /api/audit-logs` — append-only, server-generated events.
- `POST /api/backups` and `POST /api/imports` — privileged, validated operations.

## Database model

Use relational tables for users, roles, permissions, employees, suppliers, customers, projects, cost_centers, chart_of_accounts, transactions, transaction_lines, approval_events, exchange_rates, budgets, payments, assets, procurements, notifications and audit_logs. Use foreign keys for account/project/cost-center relationships and database transactions for posting.

A posted voucher should be immutable. A reversal inserts an inverse journal and marks the original as reversed. Enforce `SUM(base_debit) = SUM(base_credit)` in the database transaction, not only in the browser.

## Security requirements

- Hash passwords with a current password hashing algorithm; never store the demo `password` field.
- Use secure, HttpOnly, SameSite session cookies or short-lived access tokens with refresh rotation.
- Enforce authorization on every endpoint and workflow transition; hidden navigation is not authorization.
- Prevent self-approval and require an independent approver identity.
- Protect payroll and other confidential fields with field-level authorization.
- Write audit events server-side with old/new snapshots and request metadata.
- Validate uploads, limit file size/type, and store documents outside the web root.
- Back up the database, keys and documents; test restore procedures.

## Replacing the adapter

Keep page modules calling repository-like methods such as `DB.getAll('transactions')`, `DB.put('projects', record)` and accounting service methods. Implement those methods with `fetch('/api/...')`, return the same plain-object shapes, and remove demo seeding. Add optimistic concurrency/version checks to updates so two finance users cannot overwrite each other.
