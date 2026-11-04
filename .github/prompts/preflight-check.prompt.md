---
description: 'Preflight check for the Fleet Management System: detects the OS and verifies every required tool/runtime (Git, Python 3.13, uv, PostgreSQL, Node/npm, Angular CLI, Ionic CLI, JDK, Android SDK, Playwright) before any local development or running the app end to end. Read-only first — reports installed/missing and asks before installing anything.'
---

# Fleet Management System — Local Dev Environment Preflight Check

Before any development or running the app end to end on localhost, verify that every required tool and runtime is installed and meets the version this project needs. Do NOT install or change anything yet — first just **detect and report**.

## What to do
1. Detect my OS and shell first, then run the correct version-check command for each tool below. Run them in the integrated terminal.
2. Produce a results table with columns: **Tool | Required | Found | Status (OK / Missing / Wrong version)**.
3. For anything Missing or Wrong version, give the exact install/upgrade command for my OS (Windows). Prefer official installers or winget. **Ask me before installing anything** — never auto-install.
4. Base the required versions on `docs/PROJECT-LOG.md` (Tech Stack) and the approved stack. If a version there differs from what I list, trust the doc and note the difference.

## Tools to check

### Core
- **Git** — any recent version. `git --version`
- **VS Code** — current. `code --version`

### Backend (FastAPI + PostgreSQL)
- **Python** — 3.13.x (per PROJECT-LOG). `python --version`
- **uv** — latest (Python dependency/venv manager). `uv --version`
- **PostgreSQL** — local server + `psql` client; confirm the service is running and reachable on localhost:5432. `psql --version` and a connection test.

### Frontend (Angular web app + two Ionic mobile apps)
- **Node.js** — LTS compatible with Angular 20 (Node 20.x or 22.x). `node --version`
- **npm** — bundled with Node. `npm --version`
- **Angular CLI** — v20. `ng version`
- **Ionic CLI** — latest. `ionic --version`

### Mobile native build (Capacitor 7 → Android)
- **Java JDK** — 17+ (required by the Android Gradle toolchain). `java -version`
- **Android SDK / Android Studio** — needed to build/run the Capacitor Android apps. Check `ANDROID_HOME`/`ANDROID_SDK_ROOT` and `adb --version`.
- Note: iOS builds need macOS + Xcode and are out of scope on Windows — just flag it, don't try to fix it.

### Testing / E2E
- **Playwright** — report whether it's installed at the project level and whether its browsers are downloaded (`npx playwright --version`). Browsers are installed per-project later, so just report status.

## Rules
- Read-only first: run only version/status commands in this pass. No installs, no `npm install`, no DB changes until I say so.
- If a tool isn't found on PATH, say so explicitly rather than guessing.
- At the end, give me a short summary: **"Ready to develop"** or **"Blocked — these must be installed first: …"**, listing the blockers in priority order (runtimes before CLIs before optional mobile tooling).
- Call out anything that's installed but on a version that could cause problems (e.g. Node too new/old for Angular 20, bcrypt/passlib pin, Python not 3.13).

