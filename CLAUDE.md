# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture

Monorepo with two independent services and a shared contract:

- **`backend/`** — FastAPI + Uvicorn + Pydantic. Entry point: `backend/main.py`. Internal structure under `backend/app/` is split by domain concern: `ingestion/` → `parser/` → `graph/` → `analysis/` → `ai/`, with `api/` holding FastAPI route handlers. Data shapes live in `backend/models/`.
- **`frontend/`** — Next.js 16.2.4 + React 19 + TypeScript + Tailwind CSS v4, App Router. No Pages Router.
- **`shared/api-contract.md`** — the coordination point between frontend and backend. Fill this before writing any cross-service code.
- **`cli/`** — reserved for the `npx codeweave` package (Module 9, not yet built).

The two services communicate only over HTTP. There is no shared code between them.

## Commands

### Backend (run from repo root)
```bash
cd backend && python3 -m venv venv && source venv/bin/activate  # first-time setup
pip install -r backend/requirements.txt
uvicorn backend.main:app --reload --port 8000
```
Health check: `GET /health` → `{"status":"ok","project":"codeweave"}`

### Frontend (run from repo root)
```bash
cd frontend && npm install
npm run dev      # localhost:3000
npm run build
npm run lint     # eslint (flat config, eslint.config.mjs)
```

## Critical: Next.js 16 + React 19

**Before writing any Next.js or React code**, read `frontend/node_modules/next/dist/docs/`. Next.js 16 has breaking changes from what most training data covers. When in doubt, check those docs rather than relying on memory.

## Tailwind CSS v4

Tailwind v4 is loaded as a PostCSS plugin (`@tailwindcss/postcss`), not via `tailwind.config.js`. There is no `tailwind.config.js` — configuration is done in CSS using `@theme` and `@layer` directives inside `frontend/app/globals.css`. Do not create a `tailwind.config.js`.

## Conventions

- Backend venv lives at `backend/venv/` — activate before running any Python commands.
- No test suite exists yet. Add `pytest` to `requirements.txt` and tests under `backend/tests/` before wiring CI.
- `.env.example` at root is empty — populate it before adding any env-dependent features.
- `docker-compose.yml` exists but is empty — not used yet.
