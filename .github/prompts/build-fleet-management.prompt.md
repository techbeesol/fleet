---
description: 'End-to-end, phase-by-phase build driver for the Fleet Management System (FastAPI + PostgreSQL backend, Angular web app, two Ionic mobile apps). Reads the docs/ as source of truth and follows the implementation plan with quality gates.'
---

# Fleet Management System — End-to-End Build Kickoff

You are building the **Fleet Management System**: a multi-tenant product with a FastAPI + PostgreSQL backend, an Angular (Angular Material) Super Admin **web** app, and two Ionic + Capacitor **mobile** apps (Admin app, User app). All design decisions are already made and written down in `docs/`. Your job is to implement the whole system, phase by phase, exactly to those docs.

## STEP 0 — Read the docs before writing any code (source of truth)

Read these first and treat them as the single source of truth. Do not invent anything they already decide.

- `docs/business_requirement.md` — what the product must do.
- `docs/architecture_specification.md` — system/app boundaries, structure.
- `docs/data_model_and_api_contracts.md` — DB schema + every API contract.
- `docs/implementation_plan.md` — **the phase-by-phase build order you will follow.**
- `docs/PROJECT-LOG.md` — approved tech stack, versions, licenses, decisions.
- `docs/coding_standards.md`, `docs/validation_and_formats.md`, `docs/security_overview.md`.
- `docs/ui_design_system_and_frontend_conventions.md`, `docs/navigation_map.md`.
- `docs/configuration_and_environments.md`, `docs/api_integration_guide.md`, `docs/demo_static_data_mode.md`, `docs/user_manual.md`.
- `docs/modules/*.md` — per-module detail (dashboard, users, vehicles, trips, service, routes, transit, trip-planner, notifications). Read `docs/modules/open-questions.md` and resolve anything open before building that module.
- `agents/architect.agent.md`, `agents/developer.agent.md`, `agents/validator.agent.md` — the roles and rules you must follow.

## The three agent roles (use them)

- **architect** — plans/decides and owns the docs. Use it only if a decision is missing or must change. It never writes feature code.
- **developer** — implements each phase (backend, web, mobile) AND writes unit tests with every change.
- **validator** — runs the tests, enforces the 80% line+branch coverage gate, and does security/compliance review.

## How to run the build (follow `docs/implementation_plan.md`)

Execute the plan in dependency order, one phase at a time:

- Phase 0 — Monorepo setup & foundations (`backend/`, `frontend/super-admin-web/`, `frontend/admin-app/`, `frontend/user-app/`, `frontend/shared/`, tooling, CI, `.env.example` per app.
- Phase 1 — PostgreSQL schema via Alembic migrations + seed script.
- Phase 2 — Backend core (config, async DB, JWT auth, tenant-scoped base repository, error handling).
- Phase 3 — Auth endpoints (login/refresh/logout/me).
- Phase 4 — Backend feature modules in order (Users → Vehicles → Trips+expenses → Approval → Attachments → Services → Trip Planner → Reports), backend-first.
- Phase 5 — Shared frontend (typed models + API clients + validators/formatters; **no shared UI**).
- Phase 6 — Super Admin **web** app (Angular Material + AG Grid).
- Phase 7 — Admin & User **mobile** apps (Ionic + Capacitor; camera for receipts).
- Phase 8 — Integration, E2E & security hardening.
- Phase 9 — Deployment (Neon, Render, Cloudflare Pages, Cloudflare R2, Google Play).

For EACH phase: state the goal → implement with the **developer** role → write unit tests → run tests + coverage with the **validator** role → confirm the phase's **Exit Criteria** are met → update `docs/PROJECT-LOG.md` → only then start the next phase. Do not jump ahead or skip the gates.

## Hard rules (non-negotiable — from the agent files and plan)

1. **ASK, NEVER ASSUME.** If any requirement, field, type, role, or behavior is unclear, STOP, batch all open questions into one message, and wait for my answer. Never guess or silently default.
2. **Approved stack only** (see `docs/PROJECT-LOG.md`): FastAPI, Uvicorn, Pydantic v2, SQLAlchemy 2.0 async + asyncpg + Alembic, PyJWT, passlib[bcrypt], uv, Ruff, mypy, pytest(+cov, +asyncio), httpx, pre-commit; Angular 20, Angular Material + CDK, AG Grid Community, Bootstrap 5, Angular Signals, RxJS, ExcelJS, @ngx-translate/core; Ionic 8 + Capacitor 7, @capacitor/camera; ESLint + Prettier, Jasmine + Karma, Playwright. **Never add a dependency not on this list without asking.** Free/open-source-first.
3. **UI-kit boundary:** Angular Material = web app only. Ionic components = mobile apps only. Never mix. Share models/services/logic, never UI.
4. **Backend-first:** a feature's API + tests land before the UI that consumes it.
5. **Tenant isolation always:** every query filters by `tenant_id` from the JWT, via the central base repository. Prove isolation on every list/get.
6. **Security at the boundary:** validate all input, authorize server-side on every endpoint via a dependency, never trust the client, never log/return secrets or tokens, parameterized queries only.
7. **Thin server:** calculations (totals, KM, rollups) in SQL; services orchestrate.
8. **Tests are part of the work:** no task is done without unit tests (happy path + edge cases). Build fails below **80% line AND branch** coverage. Developer writes tests; validator runs/verifies them.
9. **Keep the log current:** after each task, append a dated entry and any new dependency to `docs/PROJECT-LOG.md`.
10. **Docs live in `docs/` only;** validation reports in `docs/validation/`. Don't scatter docs into app folders.

## Repo-specific gotchas (this workspace)

- Avoid any folder literally named `data/` (content-exclusion blocks it) — use `mock/`, `source/`, etc.
- Backend serializes **camelCase** JSON (shared TS models are camelCase) — enforce via a Pydantic alias generator in Phase 4.
- Pin bcrypt `>=4.0.1,<4.1` (passlib 1.7.4 is incompatible with bcrypt 5.x).
- `staticData` demo mode: env flag only read in the data/gateway layer; components stay mode-agnostic.
- **Never commit or push** unless I explicitly tell you to.

## Start now

Begin with **STEP 0**: read the docs listed above, then give me a short readiness summary — the stack, the phase order, and ANY open questions from `docs/modules/open-questions.md` or unclear contracts. Wait for my answers to open questions, then proceed to **Phase 0** and continue phase by phase, pausing at each phase's exit-criteria gate.