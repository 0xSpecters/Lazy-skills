# Zcode-skills

A collection of skills for AI coding agents (ZCode, Codex, Claude Code, and other SKILL.md-compatible agents). Each skill is a self-contained folder with a `SKILL.md` that provides instructions and workflows the agent loads on demand.

## Available Skills

| Skill | Description |
|-------|-------------|
| [`project-log`](skills/project-log/) | Maintains a `PROJECT_LOG.md` file in the project root that records work sessions, changes, and decisions as they happen, and produces catch-up summaries when returning to a project. |

## Installation

Install a skill with the [skills CLI](https://skills.sh):

```bash
npx skills add 0xSpecters/Zcode-skills@project-log
```

Alternatively, copy the skill folder directly into your agent's skills directory (for example `~/.zcode/skills/` or `~/.codex/skills/`):

```bash
git clone https://github.com/0xSpecters/Zcode-skills.git
cp -r Zcode-skills/skills/project-log ~/.zcode/skills/
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

## Structure

```
Zcode-skills/
└── skills/
    └── project-log/
        └── SKILL.md
```

Each `SKILL.md` follows the standard agent skill format: YAML frontmatter with `name` and `description` used for skill selection, followed by the instructions loaded when the skill is invoked.

## License

[MIT](LICENSE)
