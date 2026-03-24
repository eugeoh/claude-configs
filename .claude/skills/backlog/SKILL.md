---
name: backlog
description: 'Implement a backlog item end-to-end: plan, branch, code, test, PR. Use when the user provides a backlog item ID (e.g., SETUP-001, FE-002) to implement from docs/BACKLOG.md.'
argument-hint: '[ITEM-ID]'
disable-model-invocation: false
---

Implement backlog item `$ARGUMENTS` from `docs/BACKLOG.md`.

## Workflow

### Phase 1: Understand

1. Read `docs/BACKLOG.md` and find the item matching `$ARGUMENTS`.
2. If not found, tell the user and stop.
3. Read existing source files relevant to the ticket (check `src/`, `prisma/`, `lib/`, etc.).
4. Read surrounding code (interfaces, callers, existing tests) to understand patterns.
5. Check existing `src/` code for patterns and conventions.

### Phase 2: Plan

6. Enter plan mode. Write a concise implementation plan covering:
   - What changes are needed and where
   - New files to create (if any)
   - Test cases to add (if applicable — some infra tickets like SETUP/DEPLOY may not need tests)
7. Present the plan for lead agent approval. Do not proceed until approved.

### Phase 3: Implement

8. Create branch: `feat/$ARGUMENTS` (e.g., `feat/SETUP-001`) from `main`.
9. For tickets that have testable behavior:
   - **RED** — Write tests first (`__tests__/` or colocated `.test.ts` files). Use Vitest + React Testing Library.
   - Run `pnpm test` — verify new tests **fail**.
   - **GREEN** — Write minimal implementation to make tests pass.
   - **REFACTOR** — Clean up while keeping tests green.
10. For infrastructure/config tickets (SETUP, DEPLOY, DB schema): implement directly, verify with build/run.
11. Run `pnpm build` — fix any errors.
12. Run `pnpm test` (if tests exist) — verify all tests pass.
13. Run `pnpm lint` on changed files (if lint is configured).

### Phase 4: Review

14. Send a message to the team lead summarizing what was done and any decisions made.
15. Apply requested changes, re-run build and tests.
16. Repeat until the lead agent is satisfied.

### Phase 5: Ship

17. Commit all changes on the feature branch (do NOT include `.claude/` memory files).
18. Report completion to the team lead with a summary of what was done and any decisions made.

**Important — agents must NOT:**

- Edit `docs/BACKLOG.md` (the lead agent updates ticket statuses after merge)
- Edit `docs/SPRINT_REVIEW.md` (the lead agent writes sprint reviews)
- Merge into `main` or push to remote (the lead agent handles all merges)
- Edit files under `.claude/` (skills, memory, settings)

## Rules

- Keep changes minimal — only what the backlog item requires.
- Never log PII (resume content, user emails, etc.).
- Use `pnpm` as the package manager.
- Follow existing code patterns and project conventions.
- Follow existing patterns in `src/` — do not invent new conventions.
- When creating new files, prefer the `src/` directory structure.
- For database work, use Prisma with PostgreSQL.
- For auth, use NextAuth.js v5 (Auth.js).
- For payments, use Stripe.
- For LLM, implement the provider abstraction in `src/lib/llm/`.
