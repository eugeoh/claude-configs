# Claude Configs

Reusable [Claude Code](https://docs.anthropic.com/en/docs/claude-code) configuration — skills, settings, and development principles.

## Setup

Copy the `.claude/` directory and `CLAUDE.md` into any project root to use these configs.

## CLAUDE.md

Project-level instructions that apply to every session:

- **TDD** — Red-Green-Refactor. Write failing tests first, then implement.
- **Spec-driven development** — Start from the spec, not the code.
- **Never read `.env.local`** — Secrets are off-limits. Use `.env.example` for reference.

## Skills

### `/learn [on|off]`

Toggle interactive learning mode. When on, Claude guides you through implementation with:

- Architecture decisions with trade-off analysis
- Quiz-style concept checks before implementation
- Fill-in-the-blank coding (1-5 lines of key logic, not boilerplate)
- Links to official documentation
- Educational insights specific to the codebase

### `/backlog [ITEM-ID]`

Implement a backlog item end-to-end following TDD:

1. Read the item from `docs/BACKLOG.md`
2. Plan the implementation (requires approval)
3. Create a feature branch, write tests first, then implement
4. Build, test, lint, and report for review

### `/sprint`

Run a sprint — select up to 5 backlog items (bugs first, then by priority/size) and spawn a team of agents to implement them in parallel using git worktrees.

## Samples

The `samples/` directory (gitignored) contains test projects for trying out skills locally. See `samples/TODO.md` for a Snake game with guided learning TODOs.
