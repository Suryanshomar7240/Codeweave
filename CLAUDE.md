# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture

Monorepo with two services:

- **`frontend/`** — Next.js 16 + React 19 + TypeScript + Tailwind CSS v4, App Router
- **`backend/`** — FastAPI + Uvicorn + Pydantic, entry point at `backend/main.py`
- **`shared/api-contract.md`** — API contract between frontend and backend (not yet defined; fill this before Part 2)

The frontend and backend are independent processes. No shared code between them yet; the API contract in `shared/` is the coordination point.

## Commands

### Frontend (`cd frontend`)
```bash
npm install          # install deps
npm run dev          # dev server (localhost:3000)
npm run build        # production build
npm run lint         # eslint
```

### Backend
```bash
pip install -r backend/requirements.txt
uvicorn backend.main:app --reload   # dev server (localhost:8000)
```

Health check: `GET /health`

## Critical: Next.js 16 Breaking Changes

**Before writing any Next.js code**, read `frontend/node_modules/next/dist/docs/`. Next.js 16 has breaking API and convention changes from what most training data covers. The `frontend/AGENTS.md` enforces this rule.

## Conventions

- Tailwind CSS v4 (PostCSS plugin via `@tailwindcss/postcss`) — config differs from v3.
- No test suite configured yet. Add tests before adding a CI pipeline.
- `.env.example` exists at root but is empty — populate before adding env-dependent features.
