# Templates

Replace `{{placeholders}}` with real values. Remove sections that don't apply.

---

## CLAUDE.md

```markdown
# {{Project Name}}

{{One-sentence description}}

## Quick Start

{{commands from package.json scripts, Makefile, .csproj, etc.}}

## Documentation

Read these before making changes:

| Doc | When to read |
|-----|-------------|
| [ARCHITECTURE](.claude/ARCHITECTURE.md) | Understanding the codebase structure |
| [CONVENTIONS](.claude/CONVENTIONS.md) | Before writing or reviewing code |
| [TROUBLESHOOTING](.claude/TROUBLESHOOTING.md) | When debugging issues |
{{if frontend}}| [DESIGN_SYSTEM](.claude/DESIGN_SYSTEM.md) | Before UI work |{{/if}}
{{if complex domain}}| [BUSINESS_LOGIC](.claude/BUSINESS_LOGIC.md) | Before modifying domain logic |{{/if}}

## Key Rules

- {{Most important convention, e.g. "Server actions return { error } or { success } — never throw"}}
- {{Second rule, e.g. "All database access goes through lib/queries.ts"}}
- {{Third rule, e.g. "Use cn() for conditional Tailwind classes"}}

## Stack

{{language}} | {{framework}} | {{database}} | {{css}} | {{auth}}
```

---

## .claude/ARCHITECTURE.md

```markdown
# Architecture

## Directory Structure

{{Real tree output, annotated with brief purpose comments}}

## Data Flow

{{Describe the typical request/response cycle for the app. Examples by stack:}}

{{Next.js + Supabase:}}
1. Server Component fetches data via query helpers (`lib/queries.ts`)
2. Query helpers call Supabase client (`lib/supabase/server.ts`)
3. RLS policies filter results by authenticated user
4. Client Components handle interactivity and forms
5. Server Actions handle mutations → revalidatePath

{{React + Express:}}
1. React component calls custom hook (TanStack Query)
2. Hook calls API endpoint via fetch/axios
3. Express route handler delegates to service layer
4. Service layer handles business logic + database calls
5. Response flows back through the chain

{{.NET MVC:}}
1. Request hits Controller action
2. Controller calls Service/Repository layer
3. Entity Framework handles data access
4. ViewModel returned to View/API response

## Routing

| Route | Purpose |
|-------|---------|
| {{route}} | {{description}} |

## Key Files

| Area | Path | Purpose |
|------|------|---------|
| {{area}} | {{path}} | {{what it does}} |
```

---

## .claude/CONVENTIONS.md

```markdown
# Conventions

## Component/Class Structure

{{Show the actual pattern used in the codebase. Example for React:}}
1. Props interface
2. Hooks (useState, useEffect, useMemo, custom hooks)
3. Derived state and handlers
4. Early returns (loading, error, empty)
5. Main render

## Naming

| Thing | Convention | Example |
|-------|-----------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Utilities | camelCase | `formatDate.ts` |
| Constants | UPPER_SNAKE | `MAX_RETRIES` |
| DB columns | snake_case | `created_at` |
| CSS classes | {{Tailwind/BEM/etc.}} | {{example}} |

## Imports

{{Show the actual import order used. Example:}}
1. External packages (`react`, `next`, third-party)
2. Internal hooks and utilities
3. UI components
4. Types
5. Styles/constants

## Data Access

{{Describe how the project accesses data. Example patterns:}}
- Query helpers in `lib/queries.ts` — all reads go through here
- Server actions in `app/actions/` — all writes go through here
- Never call Supabase/DB directly from components

## Error Handling

{{Describe the actual pattern. Example:}}
- Server actions return `{ error: string }` or `{ success: true, data }`
- Never throw from server actions
- Toast notifications for user-facing errors
- Console.error for debugging

## State Management

{{Describe what the project uses and when:}}
- Server Components for data fetching (no client state needed)
- React Context for auth/theme (app-wide UI state)
- Local useState for form/component state
- {{Zustand/Redux/NgRx}} for {{specific use case}}

## Validation

{{Describe the pattern. Example:}}
- Zod schemas for form validation
- zodResolver with React Hook Form
- Server-side revalidation in server actions

## Testing

{{Describe what exists and how to run it}}
```

---

## .claude/TROUBLESHOOTING.md

```markdown
# Troubleshooting

## {{Stack}}-Specific Gotchas

{{Include only the ones relevant to the detected stack:}}

### Next.js
- `params` and `searchParams` are Promises in Next.js 15+ — must `await` them
- `revalidatePath` only works in Server Actions and Route Handlers
- Client Components (`"use client"`) cannot be async
- Hooks must come before any early returns
- Dynamic routes: `[id]` folder naming, access via `params.id`

### Supabase
- RLS policies can cause infinite recursion — use SECURITY DEFINER helper functions
- Always check `supabase.auth.getUser()` (server) not `getSession()` for auth verification
- Cookie-based auth requires `@supabase/ssr` with `cookies()` from `next/headers`
- Append 'Z' to timestamps from DB to parse as UTC

### React + Vite
- Path aliases need both `tsconfig.json` AND `vite.config.ts` configuration
- Environment variables must start with `VITE_` to be exposed to client
- HMR can cause stale closures — be careful with refs in effects

### .NET / Entity Framework
- Navigation properties need `.Include()` for eager loading
- `DbContext` is scoped — don't share across threads
- Async all the way — don't mix `.Result` with `await`
- IIS env vars may need `APPSETTING_` prefix

### Angular
- Lifecycle hooks: `ngOnInit` for setup, `ngOnDestroy` for cleanup
- Unsubscribe from Observables (use `takeUntil` or `async` pipe)
- Change detection: use `OnPush` strategy for performance
- NgRx effects must return actions (use `{ dispatch: false }` if not)

### General
- Date/timezone: always store UTC, convert to local for display only
- Financial math: use integer cents or `Math.round(amount * 100) / 100` — never raw floats
- Boolean DB columns may be stored as integers (0/1) depending on the DB
```

---

## .claude/DESIGN_SYSTEM.md (Frontend only)

```markdown
# Design System

## Stack

{{UI library}} | {{CSS framework}} | {{Icon library}} | {{Date library}}

## Colors

{{Describe the color system — semantic tokens, theme variables, or raw values}}

| Token | Usage |
|-------|-------|
| `primary` | {{buttons, links, active states}} |
| `destructive` | {{delete actions, error states}} |
| `muted` | {{disabled, secondary text}} |
| `accent` | {{highlights, badges}} |

## Components in Use

{{List the shadcn/ui / Material / etc. components actually installed and used}}

## Layout Patterns

{{Describe common layouts — sidebar+main, cards grid, form pages, etc.}}

## Spacing & Sizing

{{Describe the spacing scale, container widths, border radius conventions}}

## Accessibility

- {{Keyboard navigation requirements}}
- {{Color contrast requirements}}
- {{Screen reader considerations}}
- {{Reduced motion support}}
```

---

## .claude/BUSINESS_LOGIC.md (Complex domain only)

```markdown
# Business Logic

## Core Concepts

| Concept | Definition |
|---------|-----------|
| {{term}} | {{what it means in this domain}} |

## Key Rules

{{Document the business rules that aren't obvious from the code:}}
- {{rule 1}}
- {{rule 2}}

## Calculations

{{Document any formulas, algorithms, or non-trivial logic:}}

### {{Calculation Name}}
{{Description and formula}}

## External Integrations

| System | Purpose | Auth Method |
|--------|---------|-------------|
| {{system}} | {{what we use it for}} | {{API key / OAuth / etc.}} |

## Edge Cases

{{Document known edge cases and how they're handled}}
```
