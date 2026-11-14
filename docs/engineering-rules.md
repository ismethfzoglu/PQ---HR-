# Engineering Rules

General rules for every task on the attendance platform: **Django + DRF + PostgreSQL + Celery + Redis** backend, **React + TypeScript + Vite + Tailwind v4** frontend, multi-tenant SaaS for Iraqi businesses.

## 1. Definition of done

Every change is **enterprise-level and production ready, end to end**. A task is done only when:

- [ ] It works end to end: model → migration → service → API → frontend → UI states. No half-built layers.
- [ ] No placeholders, mock data, `TODO`, `pass`, commented-out code or "implement later" stubs.
- [ ] Errors, empty data, permissions and edge cases are handled, not just the happy path.
- [ ] Tests are written and passing (backend and frontend) for the new behaviour.
- [ ] Lint, type checks and migrations run clean.
- [ ] It follows the project docs: `docs/ui-rules.md` (all UI), the database schema and performance rules.

If any box can't be ticked, say which one and why. Never report a task as done when it isn't.

## 2. Working style

1. Read the relevant existing code before writing new code; reuse existing components, services and helpers.
2. If a requirement is ambiguous or conflicts with these rules or the docs, ask before building.
3. Change only what the task needs; no unrelated refactors or new dependencies without saying why.
4. Run the tests and checks yourself; don't assume they pass.
5. Finish with a short summary: what changed, files touched, how it was tested, anything left open.

## 3. Multi-tenancy and security (never break these)

1. **Every query is scoped by company.** Use the company-scoped manager/base queryset; a user must never see another company's data.
2. The backend enforces permissions by role (owner, HR, branch manager, platform admin); hiding UI is not security.
3. Validate all input on the backend (DRF serializers), even if the frontend validates too.
4. Device endpoint (`/iclock/`): identify the company only by a registered, active device serial number; reject unknown devices.
5. No secrets in code or in `VITE_` variables; use environment variables.
6. Session cookies `HttpOnly`, `Secure`, `SameSite=Lax`, CSRF on; never tokens in `localStorage`.
7. Never log passwords, tokens or personal data such as salaries.

## 4. Backend (Django)

1. Business logic lives in `services/` modules, not in views or serializers.
2. Every model change comes with a migration; never edit applied migrations.
3. Use `select_related` / `prefetch_related`, indexes on filtered columns, `bulk_create` for batches; no N+1 queries.
4. Anything slow (attendance recalculation, payroll, exports, WhatsApp, backups) runs in Celery, idempotent and with time limits.
5. All times stored in UTC (`USE_TZ = True`), shown in the company timezone.
6. Money is integer IQD; never floats. Payroll, social security and tax are calculated only on the backend, using `TaxRule` rates, never hardcoded.
7. API: paginated lists, consistent error format `{ field: ["message"] }`, OpenAPI schema kept up to date (drf-spectacular).
8. Raw attendance logs are never edited; corrections go through `DailyAttendance` with a reason.

## 5. Frontend (React)

1. Follow `docs/ui-rules.md` for every screen, then run its Ship checklist.
2. Pages use only `src/components/ui` components; server data only through TanStack Query and `lib/api.ts`.
3. TypeScript strict; no `any`, no `@ts-ignore`; types generated from the OpenAPI schema.
4. Every screen handles loading, empty, error and success states.
5. All text through i18n; RTL-safe logical classes; works in Arabic, dark mode and on a 360px phone.

## 6. Quality

1. **Tests:** pytest for services, APIs and permissions (including "company A can't see company B"); Vitest for frontend logic; Playwright for key flows.
2. **Performance targets:** API p95 under 200 ms; device endpoint only saves and returns `OK`; frontend first load under 2 s on 4G.
3. **Observability:** errors to Sentry; useful log messages without personal data.
4. **Code:** clear names, small functions, comments only where the reason isn't obvious.
5. **Git:** small focused commits with clear messages; never commit `.env`, secrets or build output.
