---
name: sprint
description: 'Run a sprint: pick up to 5 backlog items (bugs first), spawn a team of agents to implement them in parallel using git worktrees.'
disable-model-invocation: false
---

Run a sprint by selecting up to 5 backlog items and implementing them in parallel with a team of agents.

## Workflow

### Phase 1: Select Sprint Items

1. Read `docs/BACKLOG.md`.
2. Collect all items with status `backlog`.
3. Sort by priority:
   - **Bugs first** — all `BUG-*` tickets before other categories.
   - Then by priority: `P0` > `P1` > `P2`.
   - Then by size (smallest first): `S` > `M` > `L` > `XL`.
4. Check dependency constraints (`Depends on:` notes in ticket descriptions). Skip items whose dependencies are not `done`.
5. Select the top 5 eligible items (or fewer if less than 5 are available).
6. Present the selected items to the user and ask for confirmation before proceeding.

### Phase 2: Create Team

7. Use `TeamCreate` to create a team named `sprint`.
8. For each selected backlog item, create a task with `TaskCreate`:
   - Subject: the ticket ID and title (e.g., `BUG-010: Account deletion allows free scan farming`)
   - Description: include the ticket ID, full description from the backlog, and instructions to use the `/backlog` skill.
   - ActiveForm: `Implementing {TICKET-ID}`

### Phase 3: Spawn Agents

9. For each task, spawn a teammate agent using the `Task` tool with:
   - `subagent_type`: `general-purpose`
   - `team_name`: `sprint`
   - `name`: the ticket ID in lowercase (e.g., `bug-010`)
   - `mode`: `bypassPermissions`
   - Prompt that tells the agent to:
     a. Create a git worktree: `git worktree add ../career-lab-{TICKET-ID} -b feat/{TICKET-ID} main`
     b. Change working directory to the worktree: `cd ../career-lab-{TICKET-ID}`
     c. Use the `/backlog {TICKET-ID}` skill to implement the item end-to-end.
     d. When done, mark the task as completed.
     e. **Do NOT merge into main** — the team lead will handle merges after review.
     f. **Do NOT edit `docs/BACKLOG.md`** — the team lead will update ticket statuses.
     g. **Do NOT edit `docs/SPRINT_REVIEW.md`** or any files under `.claude/` — these are lead-only.

10. Spawn all agents in parallel (multiple `Task` tool calls in one message).

### Phase 4: Coordinate

11. As agents report back, review their work:
    - Check that builds pass (`pnpm build`)
    - Check that tests pass (`pnpm test`)
    - Verify the backlog item requirements are met
12. If changes are needed, send feedback via `SendMessage` and let the agent iterate.
13. When an agent's work is approved:
    - Merge the worktree branch into `main`: `git checkout main && git merge feat/{TICKET-ID} --no-verify && git push origin main`
    - Remove the worktree: `git worktree remove ../career-lab-{TICKET-ID}`
14. After all items are merged, clean up the team with `TeamDelete`.

### Phase 5: Sprint Review

15. Read `docs/SPRINT_REVIEW.md` to understand the existing format and determine the next sprint number.
16. Append a new sprint review entry following the established format:
    - Sprint number and date
    - Team composition (agent names)
    - Tickets Completed table (ticket ID, title, size, tests added)
    - "What Shipped" section with a brief summary of each ticket's deliverables
    - "Ready to Test" table if applicable (feature, how to test, route)
    - "Known Issues" section
17. Update the "Cumulative Progress" section at the bottom:
    - Increment completed ticket counts by category
    - Update test suite numbers if tests were added
    - Update the "Remaining P1 Backlog" table
18. Commit the sprint review update and push to main.

## Rules

- Never select more than 5 items per sprint.
- Always prioritize bug tickets over feature work.
- Always use git worktrees (`../career-lab-{TICKET-ID}`) — never work directly on `main`.
- Merge one branch at a time to avoid conflicts.
- If a merge conflict occurs, resolve it before merging the next branch.
- Stash any local changes on `main` before starting.
- **Only the lead agent** edits `docs/BACKLOG.md`, `docs/SPRINT_REVIEW.md`, and `.claude/` files. Agents must never touch these — they get overwritten on merge.
