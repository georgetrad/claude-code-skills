---
name: init-claude-docs
description: Scaffold .claude/ project documentation by analyzing the codebase. Use when starting a new project or onboarding an undocumented one.
disable-model-invocation: true
---

# Init Claude Docs

Generate `.claude/` documentation and a root `CLAUDE.md` for a project by analyzing its codebase.

## Phase 1: Codebase Analysis

Before generating anything, silently analyze:

1. **Detect stack** — Read `package.json`, `*.csproj`, `pyproject.toml`, `go.mod`, `Cargo.toml`, or equivalent. Identify:
   - Language & runtime (Node, .NET, Python, Go, Rust)
   - Framework (Next.js, React+Vite, Angular, ASP.NET, FastAPI, etc.)
   - UI library (shadcn/ui, Material, Bootstrap, none)
   - Database (Supabase, Prisma, EF Core, raw SQL, none)
   - Auth (Supabase Auth, MSAL/Azure AD, OIDC, JWT, none)
   - State management (Zustand, NgRx, Redux, Context, none)
   - Testing (Jest, Vitest, pytest, xUnit, none)
   - CSS approach (Tailwind, CSS Modules, styled-components, SCSS)

2. **Map structure** — Run `find . -type f -not -path '*/node_modules/*' -not -path '*/.git/*' -not -path '*/dist/*' -not -path '*/bin/*' -not -path '*/obj/*' | head -200` to understand the directory layout.

3. **Identify key files** — Read entry points, config files, and a few representative source files to understand patterns in use (component structure, data access, routing, error handling).

4. **Check existing docs** — Look for existing `CLAUDE.md`, `.claude/`, `README.md`, `CONTRIBUTING.md`. If `CLAUDE.md` already exists, ask the user if they want to overwrite or augment.

## Phase 2: Generate Documentation

Create these files using the templates in [references/templates.md](references/templates.md). Replace all `{{placeholders}}` with values from your analysis. Remove any sections that don't apply to the detected stack.

### Files to generate:

| File | Purpose | When to include |
|------|---------|-----------------|
| `CLAUDE.md` | Entry point — quick start, doc map, key rules | Always |
| `.claude/ARCHITECTURE.md` | Directory structure, data flow, routing | Always |
| `.claude/CONVENTIONS.md` | Coding patterns, naming, component structure | Always |
| `.claude/TROUBLESHOOTING.md` | Known gotchas for the detected stack | Always |

### Conditional files:

| File | When to include |
|------|-----------------|
| `.claude/DESIGN_SYSTEM.md` | Frontend projects with UI components |
| `.claude/BUSINESS_LOGIC.md` | Projects with complex domain rules (financial calculations, multi-step workflows, integrations) |

Do NOT generate:
- `REQUIREMENTS.md` — that's for the user to write
- `CHANGELOG.md` — that accumulates over time
- Plans or bug files — those are session-specific

## Phase 3: Populate with Real Data

This is critical — **do not generate generic boilerplate**. Every section must contain actual values from the analysis:

- `ARCHITECTURE.md` directory tree must reflect the real file structure
- `CONVENTIONS.md` patterns must match what the code actually does (read 3-5 source files to verify)
- `TROUBLESHOOTING.md` should include stack-specific gotchas from [references/templates.md](references/templates.md) that apply
- `CLAUDE.md` commands must be the real commands from package.json/Makefile/etc.

## Rules

- Keep files concise. Aim for 40-100 lines per file. Brevity saves context tokens.
- Use tables and bullet points over prose.
- Include code examples only when the pattern isn't obvious.
- If unsure about a convention, read more source files rather than guessing.
- Prefer showing the project's actual patterns over prescribing ideal ones.
