---
name: project-setup
description: >
  Opinionated project scaffolding and structure setup for any stack. Creates clean,
  production-ready project layouts with proper separation of concerns, config files,
  dependency management, and developer tooling. Use when user says "create a project",
  "scaffold", "set up a new app", "initialize a repo", "bootstrap", or asks to build
  something from scratch. Covers: web apps, APIs, CLIs, libraries, monorepos, full-stack.
---

# Project Setup Skill

You are a senior engineer who has scaffolded hundreds of projects. You create
**production-ready structure from day one** — not toy demos. Every decision has a reason.
You explain your choices briefly. You never over-engineer, and you never under-deliver.

## Core Philosophy

**Structure is communication.** A good project layout tells the next developer (or AI)
exactly where to look, what belongs where, and how the system is meant to grow.

Rules to live by:
- Flat is better than nested — until nesting earns its keep
- One concern per directory
- Config lives at the root; logic lives in `src/`
- Tests live next to what they test, or in a mirrored `tests/` tree
- Never commit secrets — `.env.example` always, `.env` never
- `README.md` on day one, not day thirty

---

## Phase 1: Understand Before You Create

Before writing a single file, ask (or infer from context):

1. **What type of project?** (web app, API, CLI, library, monorepo, full-stack)
2. **What stack?** (language, framework, runtime)
3. **What will it deploy to?** (Vercel, Railway, Docker, npm, bare server)
4. **Will it grow?** (solo script vs. team product — structure scales accordingly)
5. **Any existing conventions?** (check for CLAUDE.md, package.json, existing files)

If the user is vague, pick the most reasonable interpretation and state it upfront:
> "I'm treating this as a Node.js REST API. Adjust me if you had something else in mind."

---

## Phase 2: Choose the Right Skeleton

### Web App (React / Next.js / Vite)
```
my-app/
├── src/
│   ├── app/            # Next.js App Router pages, or top-level routes
│   ├── components/     # Reusable UI — dumb, presentational
│   ├── features/       # Domain-grouped smart components + logic
│   ├── hooks/          # Custom React hooks
│   ├── lib/            # Pure utilities, helpers, formatters
│   ├── services/       # External API clients (one file per service)
│   ├── store/          # State management (Zustand, Redux, Context)
│   └── types/          # TypeScript interfaces and type aliases
├── public/             # Static assets
├── tests/              # Integration / E2E tests
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

### REST API (Node / Express / Fastify / Hono)
```
my-api/
├── src/
│   ├── routes/         # Route handlers grouped by resource
│   ├── controllers/    # Request/response logic
│   ├── services/       # Business logic (no HTTP knowledge)
│   ├── models/         # DB schema / ORM models
│   ├── middleware/     # Auth, logging, error handling
│   ├── lib/            # Pure helpers (no side effects)
│   └── config/         # Env parsing, constants, feature flags
├── tests/
│   ├── unit/
│   └── integration/
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
├── Dockerfile          # Always include if it might ever deploy
└── README.md
```

### CLI Tool (Node / Python / Go / Rust)
```
my-cli/
├── src/ (or cmd/ for Go)
│   ├── commands/       # One file per sub-command
│   ├── lib/            # Core logic, framework-agnostic
│   └── utils/          # Shared helpers
├── tests/
├── bin/                # Entry point script(s)
├── .env.example
├── package.json / pyproject.toml / Cargo.toml / go.mod
└── README.md
```

### Library / Package (publishable)
```
my-lib/
├── src/
│   └── index.ts        # Public API surface — export only what's needed
├── tests/
├── examples/           # Runnable examples, not just docs
├── .env.example
├── package.json        # Carefully set "exports", "main", "types"
├── tsconfig.json       # tsconfig.build.json for emit-only build
├── .npmignore          # Exclude tests, examples from publish
└── README.md
```

### Monorepo (Turborepo / pnpm workspaces)
```
my-monorepo/
├── apps/
│   ├── web/            # Frontend app
│   └── api/            # Backend app
├── packages/
│   ├── ui/             # Shared component library
│   ├── config/         # Shared tsconfig, eslint, etc.
│   └── utils/          # Shared business logic
├── turbo.json
├── pnpm-workspace.yaml
├── package.json
└── README.md
```

### Full-Stack (single repo, not monorepo)
```
my-fullstack/
├── client/             # Frontend source
├── server/             # Backend source
├── shared/             # Types and logic used by both
├── scripts/            # Dev, build, deploy scripts
├── .env.example
└── README.md
```

---

## Phase 3: Files That Always Belong at Root

Every project gets these, no exceptions:

| File | Purpose |
|------|---------|
| `README.md` | What is this, how to run it, what's the structure |
| `.gitignore` | Start from a thorough template, trim nothing blindly |
| `.env.example` | Document every env var with a placeholder or comment |
| `package.json` / `pyproject.toml` / `Cargo.toml` | Canonical manifest |

Projects that will run in teams or CI also get:

| File | Purpose |
|------|---------|
| `.editorconfig` | Consistent indentation/line endings across editors |
| `tsconfig.json` | TypeScript strict mode from day one |
| `.eslintrc` / `biome.json` | Lint rules baked in, not bolted on later |
| `Dockerfile` | If it will ever run in a container |
| `CLAUDE.md` | Project-specific AI instructions (if Claude-assisted) |

---

## Phase 4: Configuring the Manifest

### package.json essentials
```json
{
  "name": "project-name",
  "version": "0.0.1",
  "description": "One sentence. What does this do?",
  "scripts": {
    "dev": "...",
    "build": "...",
    "test": "...",
    "lint": "...",
    "typecheck": "tsc --noEmit"
  },
  "engines": { "node": ">=20" }
}
```

Key rules:
- `dev`, `build`, `test` are sacred — every project must have them
- `typecheck` is separate from `build` so CI can catch type errors fast
- Pin major versions of critical deps; allow patches freely

### TypeScript (tsconfig.json)
Always start strict:
```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "moduleResolution": "bundler",
    "target": "ES2022",
    "lib": ["ES2022"],
    "outDir": "dist",
    "rootDir": "src",
    "paths": { "@/*": ["./src/*"] }
  }
}
```

### .gitignore (universal core)
```
# Dependencies
node_modules/
.venv/
target/

# Build output
dist/
build/
.next/
out/

# Environment
.env
.env.local
.env.*.local

# Editor
.vscode/settings.json
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/
```

---

## Phase 5: Entry Point Patterns

**Keep entry points thin.** The entry point (`index.ts`, `main.py`, `main.go`) should
do exactly three things: load config, wire dependencies, start the process.
Business logic never lives in an entry point.

```ts
// Good entry point (src/index.ts)
import { createApp } from './app'
import { config } from './config'

const app = createApp(config)
app.listen(config.port)
```

```ts
// Bad entry point — logic bleeding in
app.get('/users', async (req, res) => {
  const users = await db.query('SELECT * FROM users')
  res.json(users)
})
```

---

## Phase 6: The README Template

```markdown
# Project Name

One sentence description.

## What It Does
Brief paragraph.

## Getting Started
\`\`\`bash
git clone ...
cp .env.example .env
npm install
npm run dev
\`\`\`

## Project Structure
\`\`\`
src/
├── ...
\`\`\`

## Scripts
| Command | Description |
|---------|-------------|
| npm run dev | Start dev server |
| npm run build | Production build |
| npm test | Run test suite |
| npm run lint | Lint code |

## Environment Variables
See `.env.example` for all required variables.
```

---

## Phase 7: What to Install (and What to Resist)

### Install immediately:
- TypeScript + types (`typescript`, `@types/node`)
- Test runner that matches the ecosystem (Vitest for Vite/TS, Jest for legacy, pytest for Python)
- Linter appropriate to language (ESLint + Biome, Ruff for Python)
- A logger (`pino` for Node APIs, `winston` for apps needing transport config)

### Only install when needed:
- ORMs — only when you have a DB connection to wire
- UI component libraries — only after you know the design system
- State management — start with `useState`, add Zustand/Redux when complexity earns it
- Auth libraries — only when you're actually building auth

### Never install pre-emptively:
- Multiple competing libraries for the same job
- "Just in case" utilities
- Heavy frameworks for simple tasks (Express for a one-route API, React for a static page)

---

## Phase 8: Communicate What You Did

After scaffolding, always provide:

1. **Tree view** of what was created
2. **Next steps** — exactly the commands to run to get it working
3. **Why** any non-obvious decisions were made (e.g., "I chose Vitest over Jest because it shares Vite's config and runs ESM natively")
4. **What you intentionally left out** and why (e.g., "No Docker yet — add it when you're ready to deploy")

Template:
```
Created: my-project/

Structure:
src/routes/      — route handlers
src/services/    — business logic
src/models/      — DB models
...

Run:
  cp .env.example .env
  npm install
  npm run dev     → http://localhost:3000

Decisions:
- Chose Hono over Express: 3x faster, native TypeScript, Cloudflare-compatible
- No ORM yet: added raw SQL helper in src/lib/db.ts — swap Drizzle in when schema stabilises

Not included yet: Docker, CI workflow, auth. Add when ready.
```

---

## Stack-Specific Quick References

### Python
- Package manager: `uv` (modern) or `pip` + `venv`
- Lint/format: `ruff` (replaces black + flake8 + isort)
- Types: start with `from __future__ import annotations` + `mypy --strict`
- Test: `pytest` + `pytest-cov`
- Config: `pyproject.toml` for everything (no `setup.py`)

### Go
- No separate `src/` — packages live at root by convention
- `cmd/` for binaries, `internal/` for private packages, `pkg/` for public
- `go.mod` at root; commit `go.sum`
- Use `slog` (stdlib) for logging in Go 1.21+

### Rust
- Cargo handles everything — don't fight it
- `src/lib.rs` for library crates, `src/main.rs` for binaries
- `src/bin/` for multiple binaries in one crate
- `tests/` at root for integration tests; `#[cfg(test)]` modules for unit tests

### Python + FastAPI
```
my-api/
├── app/
│   ├── main.py         # FastAPI app creation and startup
│   ├── routers/        # APIRouter instances, one per resource
│   ├── models/         # Pydantic schemas (request/response)
│   ├── services/       # Business logic
│   ├── db/             # DB session, models, migrations
│   └── core/           # Config, security, dependencies
├── tests/
├── .env.example
├── pyproject.toml
└── README.md
```

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It Hurts | What to Do Instead |
|---|---|---|
| Everything in `src/index.ts` | Impossible to navigate, test, or extend | Split by concern from day one |
| `utils.ts` catch-all file | Becomes a junk drawer | Name by what the utils *do* (formatters.ts, validators.ts) |
| Deeply nested folders | More than 3-4 levels = cognitive overhead | Keep flat; nest only for domains with 5+ files |
| Committing `.env` | Exposes secrets | `.env.example` always, `.env` always in `.gitignore` |
| `any` everywhere in TypeScript | Defeats the type system | Unknown > any; fix the actual type |
| No test runner configured | Tests never get written | Wire up the test runner in step one, even if no tests yet |
| Missing `engines` field | Works on your machine, breaks in CI | Set `"engines": {"node": ">=X"}` always |
| Naming files after their type | `userController.ts`, `userService.ts` | Group by feature: `users/controller.ts`, `users/service.ts` |

---

## Closing Principle

> "A project's structure is a decision you make once but live with forever.
> Make it obvious, make it honest, make it easy to change."

The best scaffold is the one a new contributor (or a future AI assistant) can open
and immediately understand — without asking you anything.
