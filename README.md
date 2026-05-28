# Pavan Project Starter

A reusable Project OS for serious software projects.

This repository is a starter kit you can copy into new projects before writing application code. It gives each project a repeatable operating system for AI-assisted planning, implementation, review, testing, documentation, and safety checks.

The core idea is simple:

```text
Do not rely on memory.
Do not rely on one giant prompt.
Save the workflow into files that travel with every project.
```

## What This Repository Is

This repo is not an application. It is a template for applications.

Use it when starting projects such as:

- Startup or portfolio apps
- Hackathon projects
- Legal document generators
- Finance or data validation products
- LLM-powered tools
- Backend/frontend prototypes that may grow into serious systems

It gives a new project:

- AI coding instructions
- Claude Code project memory
- Project-specific Claude subagents
- Cursor rules
- Documentation templates
- Safety hooks
- Pre-commit hooks
- GitHub Actions CI
- Bootstrap and validation scripts

## The Three-Layer Model

This starter kit is designed around three layers.

### 1. Global Personal Setup

Global files live outside any individual project.

For Claude Code, reusable agents live in:

```text
~/.claude/agents/
```

These are generic agents you want available everywhere:

- `code-review-agent.md`
- `debugging-agent.md`
- `security-agent.md`
- `architecture-agent.md`
- `testing-agent.md`

Use this layer for rules and agents that should apply to every project you work on.

### 2. Template Repo

This repository is the template layer.

Copy this repo into each new project:

```bash
cp -R ~/dev/project-starter-kit/. my-new-project/
cd my-new-project
git init
bash scripts/bootstrap_project.sh
```

This gives the new project the full starter structure: docs, AI instructions, hooks, agents, Cursor rules, CI, and utility scripts.

### 3. Project-Specific Customisation

After copying the template, adapt the project-level files for that specific product.

Examples:

- A legal document generator may add agents for DOCX XML fidelity, placeholder schemas, and golden-output testing.
- A finance product may add agents for data integrity, backtesting, and model versioning.
- A social/community product may add agents for safety, notification flows, and Supabase RLS.

Project-specific agents live in:

```text
.claude/agents/
```

These files should be checked into the project repo when they are useful to teammates.

## Repository Structure

```text
project-starter-kit/
  AGENTS.md
  CLAUDE.md
  PROJECT_STARTER_SYSTEM.md
  README.md

  docs/
    PROJECT_BRIEF.md
    PROJECT_STATUS.md
    ARCHITECTURE.md
    DATA_MODEL.md
    API_CONTRACT.md
    TESTING.md
    DECISIONS.md
    SECURITY_REVIEW.md

  .claude/
    settings.json
    agents/
      product-requirements-agent.md
      architecture-agent.md
      data-model-agent.md
      api-contract-agent.md
      security-agent.md
      testing-agent.md
      code-review-agent.md
      debugging-agent.md
      documentation-agent.md
      startup-validation-agent.md
    hooks/
      protect-files.sh
      format-after-edit.sh
      run-targeted-checks.sh

  .cursor/
    rules/
      project-core.mdc
      backend-fastapi.mdc
      frontend-nextjs.mdc
      security.mdc
      testing.mdc

  .github/
    workflows/
      ci.yml

  scripts/
    bootstrap_project.sh
    run_checks.sh
    run_tests.sh
    validate_env.sh

  .pre-commit-config.yaml
  .gitignore
  .env.example
```

## Root Files

### `AGENTS.md`

Vendor-neutral AI coding instructions.

Use this as the first file any AI coding assistant reads. It explains the required project docs, working rules, testing expectations, security review triggers, and documentation-update requirements.

Important rules include:

- Read project docs before coding.
- Do not invent environment variables.
- Do not remove tests to make builds pass.
- Update project status after meaningful work.
- Run security review for sensitive features.

### `CLAUDE.md`

Claude-specific project context.

This file tells Claude Code how to work in the repo, including preferred stack, common risks, and the expected workflow before implementation.

It is intentionally practical: read the docs, understand the product, check existing patterns, make a plan, then implement.

### `PROJECT_STARTER_SYSTEM.md`

The full startup workflow for a new serious project.

It defines seven phases:

1. Product
2. Architecture
3. Data model
4. API contract
5. Testing
6. Security
7. Coding

The key rule is that coding starts only after the planning docs exist.

## Documentation Templates

The `docs/` directory is the project memory layer.

Each file captures one part of the system so future AI sessions and future developers can quickly recover context.

### `docs/PROJECT_BRIEF.md`

Defines the product.

Includes:

- One-line description
- Problem
- Target users
- User journeys
- MVP scope
- Non-MVP scope
- Success metrics
- Key risks
- Open questions

### `docs/PROJECT_STATUS.md`

The running status file for every project.

This is one of the most important files in the starter kit. It gives future sessions an immediate snapshot of what works, what is in progress, known issues, current architecture, latest decisions, next tasks, and things that must not break.

Update this after meaningful implementation work.

### `docs/ARCHITECTURE.md`

Describes the technical shape of the system.

Includes:

- Recommended stack
- Folder structure
- Service boundaries
- Core flows
- API shape
- Data flow
- Deployment assumptions
- Tradeoffs

### `docs/DATA_MODEL.md`

Defines entities and persistence rules.

Includes:

- Entities
- Fields
- Relationships
- Validation rules
- Indexes
- Example payloads
- Data integrity rules
- Migration notes

### `docs/API_CONTRACT.md`

Defines the backend/frontend contract.

Includes:

- Auth scheme
- Required headers
- Error format
- Routes
- Request bodies
- Response bodies
- Status codes
- API compatibility rules

### `docs/TESTING.md`

Defines the testing strategy.

Includes:

- Unit tests
- Integration tests
- API tests
- Frontend tests
- End-to-end tests
- Regression tests
- Edge cases
- Manual verification
- Test commands

### `docs/DECISIONS.md`

Records important product and architecture decisions.

Use it when choosing frameworks, database models, auth strategy, deployment architecture, LLM boundaries, file-processing behavior, or any tradeoff that future maintainers should understand.

### `docs/SECURITY_REVIEW.md`

Tracks security risks and mitigations.

Use this especially for:

- Auth
- Payments
- Legal workflows
- Financial workflows
- Health data
- Personal data
- File uploads
- LLM features
- Secrets handling

## Claude Code Setup

### `.claude/settings.json`

Claude Code settings for the project.

This file:

- Denies reads of sensitive files such as `.env`, `.env.local`, `.env.production`, secrets, and credentials.
- Runs a protected-file hook before edits.
- Runs formatting and targeted checks after edits.

The settings allow `.env.example` to be read because agents need it to understand expected environment variables.

### `.claude/agents/`

Project-level Claude subagents.

These are specialized Markdown files with YAML frontmatter. Claude uses each agent's `description` to decide when to delegate work.

Included agents:

- `product-requirements-agent.md`: turns an idea into a scoped product brief.
- `architecture-agent.md`: designs stack, boundaries, folder structure, and deployment assumptions.
- `data-model-agent.md`: designs entities, fields, validation rules, indexes, and examples.
- `api-contract-agent.md`: defines routes, schemas, errors, and auth requirements.
- `security-agent.md`: reviews sensitive flows, secrets, LLM risks, file uploads, and data handling.
- `testing-agent.md`: designs unit, integration, API, frontend, edge-case, and regression tests.
- `code-review-agent.md`: reviews code changes for bugs, security, maintainability, and missing tests.
- `debugging-agent.md`: investigates failing tests, builds, ports, Docker issues, and runtime bugs.
- `documentation-agent.md`: updates docs, status, decisions, and developer-facing explanations.
- `startup-validation-agent.md`: validates startup, portfolio, and hackathon project scope.

### `.claude/hooks/`

Automation scripts used by Claude Code hooks.

#### `protect-files.sh`

Blocks edits to protected files and directories.

Protected examples:

- `.env`
- `.env.local`
- `.env.production`
- `.git/`
- `node_modules/`
- lockfiles
- migration directories
- golden outputs
- base templates

This is intentionally conservative. You can adjust it per project.

#### `format-after-edit.sh`

Formats edited files when local tools are available.

Behavior:

- Python: runs `ruff format` and `ruff check --fix`
- TypeScript, JavaScript, JSON, Markdown, CSS, YAML: runs `npx prettier --write`

If the tools are not installed, the script exits without failing the workflow.

#### `run-targeted-checks.sh`

Runs lightweight checks after edits.

Behavior:

- Python: runs `python -m py_compile`
- JSON: runs `jq empty`
- YAML: uses Python YAML parsing if `PyYAML` is installed

This is not a replacement for full tests. It is a fast guardrail.

## Cursor Setup

Cursor project rules live in:

```text
.cursor/rules/
```

Included rules:

- `project-core.mdc`: always-on core AI coding behavior.
- `backend-fastapi.mdc`: FastAPI backend conventions.
- `frontend-nextjs.mdc`: Next.js frontend conventions.
- `security.mdc`: security rules for sensitive features.
- `testing.mdc`: testing rules and regression-test expectations.

These rules help Cursor follow the same workflow as Claude Code.

## Scripts

### `scripts/bootstrap_project.sh`

Initializes a newly copied project.

It creates expected directories and docs, ensures `.env.example` exists, creates a default `.gitignore` if missing, and installs pre-commit hooks when `pre-commit` is available.

Run it after copying the starter kit:

```bash
bash scripts/bootstrap_project.sh
```

### `scripts/run_checks.sh`

Runs local quality checks.

It attempts:

- `pre-commit run --all-files`
- `ruff check .`
- `ruff format --check .`
- `npm run lint --if-present`
- `npm run typecheck --if-present`

Missing tools are skipped cleanly.

### `scripts/run_tests.sh`

Runs project tests.

It attempts:

- `pytest` when a `tests/` directory exists
- `npm test --if-present` when `package.json` exists

### `scripts/validate_env.sh`

Compares `.env` against `.env.example`.

It checks that every key in `.env.example` exists in `.env`. It does not print secret values.

## Pre-Commit

The root `.pre-commit-config.yaml` includes:

- YAML checks
- JSON checks
- End-of-file fixes
- Trailing-whitespace cleanup
- Large-file checks
- Ruff check and format hooks

Install hooks in a new project:

```bash
pre-commit install
pre-commit install -t pre-push
pre-commit run --all-files
```

## GitHub Actions CI

The workflow in `.github/workflows/ci.yml` runs on pushes and pull requests.

It has two jobs:

- `backend-checks`: Python setup, dependency installation, pre-commit, and pytest when tests exist.
- `frontend-checks`: Node setup, package install when lockfiles exist, lint, typecheck, and tests when package scripts exist.

The workflow is intentionally generic so it can run before a project has a full app structure.

## How To Start A New Project

Copy the starter:

```bash
mkdir my-new-project
cp -R ~/dev/project-starter-kit/. my-new-project/
cd my-new-project
git init
bash scripts/bootstrap_project.sh
```

Then open the project in Claude Code or Cursor and start with:

```text
Read AGENTS.md, CLAUDE.md, and PROJECT_STARTER_SYSTEM.md.

This project is: [describe it].

Do not write app code yet.

Generate:
1. docs/PROJECT_BRIEF.md
2. docs/ARCHITECTURE.md
3. docs/DATA_MODEL.md
4. docs/API_CONTRACT.md
5. docs/TESTING.md
6. docs/SECURITY_REVIEW.md
7. docs/DECISIONS.md

After that, give me the implementation sequence.
```

## Recommended Workflow

```text
Idea
  |
Copy project-starter-kit
  |
Fill PROJECT_BRIEF.md
  |
Use architecture-agent
  |
Use data-model-agent
  |
Use api-contract-agent
  |
Use testing-agent
  |
Use security-agent
  |
Start coding
  |
Run hooks + tests
  |
Use code-review-agent
  |
Update PROJECT_STATUS.md
```

## Project Size Guidance

For a small weekend project, use:

- `AGENTS.md`
- `docs/PROJECT_STATUS.md`
- Pre-commit
- Basic CI

For a serious portfolio or startup project, use:

- Full starter kit
- Agents
- Hooks
- CI
- Docs
- Security review
- Testing plan

For legal, finance, health, or LLM projects, add:

- Extra security review
- Data integrity review
- Golden tests where outputs must be stable
- Disclaimers where appropriate
- Audit logs where appropriate

## Customizing The Starter

After copying this template into a real project:

1. Replace placeholder docs with real project context.
2. Update `.env.example` with real required variables.
3. Remove unused Cursor rules if the stack differs.
4. Add project-specific Claude agents under `.claude/agents/`.
5. Adjust protected patterns in `.claude/hooks/protect-files.sh`.
6. Update CI once the real dependency manager and test commands are known.
7. Keep `docs/PROJECT_STATUS.md` current after every major change.

## Design Principles

- Documentation first, then implementation.
- Small changes over broad rewrites.
- Explicit contracts over assumptions.
- Tests for backend behavior and bug fixes.
- Security review for sensitive domains.
- Project status as the handoff point between AI sessions.
- Agents and hooks as reusable workflow infrastructure.
