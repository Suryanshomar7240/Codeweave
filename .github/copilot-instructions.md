<!--
Guidance for AI coding agents working in this repository.
This file is intentionally concise and actionable. If the repository has source files,
inspect the filenames referenced below and run the detection steps to discover
the project's language, build and test commands, and architecture boundaries.
-->

# .github/copilot-instructions.md

Follow these steps to get productive quickly in this codebase:

1. Project discovery (always do first)
   - Look for root manifests: `package.json`, `pyproject.toml`, `requirements.txt`, `pom.xml`, `go.mod`, `Cargo.toml`.
   - If found, read scripts/build/test sections to infer commands (e.g. `npm test`, `mvn -DskipTests=false test`, `pytest`).
   - Check for `README.md`, `.env.example`, `Dockerfile`, `docker-compose.yml`, and `.github/workflows` for CI patterns.

2. Architecture & file layout heuristics (examples you will see)
   - Single-service JS/TS web app: `package.json` + `src/`, `public/`, `tests/`.
   - Python app: `pyproject.toml` or `requirements.txt` + `src/` or top-level `app/`, `tests/`, `manage.py` or `main.py`.
   - Multi-service monorepo: look for `services/` or `packages/` with own manifests.
   - Serverless: `functions/` or `lambda/` folders and provider configs (serverless.yml).

3. Common quick commands (detect file before running)
   - Node: `npm ci && npm test` (or `pnpm install && pnpm test`) if `package.json` exists.
   - Python: create venv -> `pip install -r requirements.txt` -> `pytest -q` when `requirements.txt` or `pyproject.toml` present.
   - Java: `mvn -q test` if `pom.xml` present.
   - Docker: `docker build -t app .` if `Dockerfile` present; inspect `docker-compose.yml` for multi-service runs.

4. Project-specific conventions to preserve
   - Prefer reading `README.md` first for any project-specific scripts. If tests are wired to a script name (e.g., `ci:test` or `test:unit`), use that exact script.
   - Environment/config: look for `.env`, `.env.example`, or `config/*.yaml` and never print secrets; if secrets are missing, prompt the user.

5. Integration points to check
   - DB connection strings or providers (look for `DATABASE_URL`, `mongo`, `postgres`, `redis`).
   - Cloud infra: check `infra/`, Terraform/Bicep, or `azd/`, `.github/workflows/*` for deployment steps.
   - External APIs: config files with keys/URLs and SDK usage in source (e.g., `aws-sdk`, `@google-cloud/*`).

6. When editing or adding tests/code
   - Keep changes minimal and focused. Add or update unit tests adjacent to code under `tests/` or `__tests__/`.
   - Run the project's test command before proposing a PR. If tests fail locally, report failures and stack traces concisely.

7. If the repository is empty or missing expected files
   - Do not guess project internals. Create a short issue or ask the repository owner for the missing artifacts (README, manifest, sample env).
   - Provide a recommended checklist of files to add (manifest, README, simple test) before continuing.

8. Examples of actionable checks for this repo (replace placeholders after discovery)
   - If `package.json` exists: open top-level `scripts` and use exact script names (e.g., `npm run lint`, `npm run test:unit`).
   - If `pyproject.toml` exists: read `[tool.poetry.scripts]` or `tool.pytest.ini_options` to learn test patterns.

9. Communication style
   - When uncertain, ask one targeted question (e.g., "Which DB should tests use: sqlite in-memory or a running Postgres?").
   - Keep suggestions small and reversible. Use the repository's style and test rules.

If anything above is unclear or you want this file adapted to the actual repository layout, provide the repository files (or grant access) and I will merge repository-specific examples into this document.
