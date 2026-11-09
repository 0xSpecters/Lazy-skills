# Lazy-skills

A collection of skills for AI coding agents (ZCode, Codex, Claude Code, and other SKILL.md-compatible agents). Each skill is a self-contained folder with a `SKILL.md` that provides instructions and workflows the agent loads on demand.

## Available Skills

| Skill | Description |
|-------|-------------|
| [`project-log`](skills/project-log/) | Maintains a `PROJECT_LOG.md` file in the project root that records work sessions, changes, and decisions as they happen, and produces catch-up summaries when returning to a project. |
| [`prompt-maker`](skills/prompt-maker/) | Rewrites a messy or underspecified instruction into an optimized, project-grounded `PROMPT.md` instruction file, then executes the task from that file as the single source of truth. |

## Installation

Install a skill with the [skills CLI](https://skills.sh):

```bash
npx skills add 0xSpecters/Lazy-skills@project-log
npx skills add 0xSpecters/Lazy-skills@prompt-maker
```

Alternatively, copy the skill folder directly into your agent's skills directory (for example `~/.zcode/skills/` or `~/.codex/skills/`):

```bash
git clone https://github.com/0xSpecters/Lazy-skills.git
cp -r Lazy-skills/skills/project-log ~/.zcode/skills/
cp -r Lazy-skills/skills/prompt-maker ~/.zcode/skills/
```

## Usage

### project-log

The skill operates in two modes:

- **Session tracking** — While working in a project, the agent keeps `PROJECT_LOG.md` at the project root current: an entry per session with the goal, changes made (with file paths), decisions with rationale, and pending next steps. Updates are incremental rather than batched at the end of the session.
- **Catch-up summary** — When returning to a project, the agent reads the log, cross-checks it against git history and recently modified files, and reports what was done, what is unfinished, and the recorded next steps.

Invoking it explicitly as `/project-log` starts a session entry; asking the agent "what was I working on here?" triggers the catch-up summary.

Example log structure:

```markdown
## Session — 2026-10-31
**Goal:** Add pagination to the users API

### Changes
- Added cursor-based pagination to src/api/users.ts

### Decisions & notes
- Chose cursor pagination over offset: the users table is expected to exceed 1M rows

### Next steps
- [ ] Add pagination metadata to the response envelope
```

### prompt-maker

The skill activates on explicit invocation (`/prompt-maker`) or when a prompt is explicitly asked to be optimized or structured. It does not trigger on ordinary instructions.

The workflow has five stages:

1. **Project scan** — Reads README, agent instruction files, manifests, directory structure, and git state, extracting only facts relevant to the task.
2. **Instruction analysis** — Separates the prompt into intent, explicit requirements, implicit assumptions, and ambiguities. Blocking questions are asked before proceeding; non-blocking ones are recorded with the assumption being made.
3. **Instruction file** — Writes `PROMPT.md` at the project root with Goal, Context, Requirements, Non-goals, Assumptions, Plan, Verification, and Status sections. An unrelated existing `PROMPT.md` is never overwritten.
4. **Execution** — The agent works from the file, keeping Status current. Any deviation from the plan is recorded in the file before continuing.
5. **Wrap-up** — Status is set to Done and the exercised assumptions are reported. The file remains as a record of the interpretation.

Example instruction file:

```markdown
<!-- prompt-maker: 2026-11-10 -->

# Add search to the users table

## Goal
Users can filter the users table by name or email, debounced client-side.

## Requirements
1. Single text input above the table, filters both name and email columns
2. 300 ms debounce before the query updates
3. Empty query shows all rows

## Non-goals
- Server-side search
- Filter by role or status

## Assumptions
- Assumed client-side filtering is acceptable for the current dataset size (<5k rows); say the word to change it.

## Plan
1. Add `useUserSearch` hook in src/hooks/useUserSearch.ts
2. Wire the input into src/pages/UsersPage.tsx
3. Verify with the dev server

## Verification
- `npm run dev`, type into the search box, confirm table filters within ~300 ms
```

## Structure

```
Lazy-skills/
└── skills/
    ├── project-log/
    │   └── SKILL.md
    └── prompt-maker/
        └── SKILL.md
```

Each `SKILL.md` follows the standard agent skill format: YAML frontmatter with `name` and `description` used for skill selection, followed by the instructions loaded when the skill is invoked.

## License

[MIT](LICENSE)
