# Claude Code Configuration

## Development Principles

### TDD (Test-Driven Development)
- Always follow Red-Green-Refactor:
  1. **RED** — Write a failing test first.
  2. **GREEN** — Write the minimal code to make the test pass.
  3. **REFACTOR** — Clean up while keeping tests green.
- Never write implementation code without a corresponding test.
- Run tests after every change.

### Spec-Driven Development
- Start from the spec. Read and understand requirements before writing any code.
- Implementation must satisfy the spec — not the other way around.
- If the spec is ambiguous, clarify with the user before proceeding.
- Keep specs and tests as the source of truth for expected behavior.

## Security

### Environment Files
- **NEVER read `.env.local`** — this file contains secrets and must not be accessed.
- Do not read, cat, grep, or otherwise access `.env.local` files under any circumstances.
- If you need to know what environment variables are available, ask the user or read `.env.example` instead.
