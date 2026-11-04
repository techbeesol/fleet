# fms
Angular Applications Docs

## Quick Start for New Developers

All design decisions are already documented under `docs/`. To build the system end to end with GitHub Copilot:

1. Open this repository in VS Code with GitHub Copilot enabled.
2. **Check your machine first:** in Copilot Chat run `/preflight-check` (`.github/prompts/preflight-check.prompt.md`). It verifies Git, Python 3.13, uv, PostgreSQL, Node/npm, Angular CLI, Ionic CLI, JDK, Android SDK, and Playwright are installed, and lists anything missing before you start.
3. In Copilot Chat, run the reusable prompt: type `/build-fleet-management` (it lives in `.github/prompts/build-fleet-management.prompt.md`).
   - If slash-prompts aren't available, open that file and paste its contents as your first chat message.
4. Copilot reads the docs, gives a readiness summary, and asks any open questions — answer them, then it builds **phase by phase** following `docs/implementation_plan.md`, pausing at each phase's exit-criteria gate.

### Where to start reading
- `docs/implementation_plan.md` — the phase-by-phase build order (start here).
- `docs/architecture_specification.md` — system and app boundaries.
- `docs/data_model_and_api_contracts.md` — database schema + API contracts.
- `docs/PROJECT-LOG.md` — approved tech stack, versions, and decisions.
- `docs/modules/` — per-module detail; check `open-questions.md` first.
- `agents/` — the architect, developer, and validator roles Copilot follows.

### Project shape
- `backend/` — FastAPI + PostgreSQL (SQLAlchemy 2.0 async + Alembic).
- `frontend/super-admin-web/` — Angular + Angular Material web app.
- `frontend/admin-app/`, `frontend/user-app/` — Ionic + Capacitor mobile apps.
- `frontend/shared/` — shared TypeScript models and API clients (no shared UI).