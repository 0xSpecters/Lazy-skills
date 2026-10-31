---
name: project-log
description: Maintain a PROJECT_LOG.md file in the project folder that records work sessions, changes made, and decisions as they happen, and produce a catch-up summary of where things left off when returning to a project. Use when starting or resuming work in a project, when the user asks what they were working on, asks to track/log session changes, or when wrapping up a session.
---

# Project Log

Maintain one persistent file, `PROJECT_LOG.md`, at the root of the project. It is the project's memory across sessions: what was done, what was decided, and what is next. The user should be able to read it alone and get back up to speed after weeks away.

## Two Modes

**Session tracking** (default while working): keep the log current as the session progresses.

- When `PROJECT_LOG.md` does not exist, create it from the template below and fill in a Project overview by skimming the codebase (README, package/manifest files, directory structure).
- Start a new `## Session` entry when work begins, with the date and a one-line goal.
- Update the entry incrementally as work happens — do not wait until the end of the session. Add to **Changes** after completing each meaningful unit of work (a feature, a fix, a refactor), and record anything in **Decisions & notes** that a future session would otherwise have to re-derive (why option A over B, non-obvious constraints, gotchas discovered).
- Record file paths, not vague descriptions. `Added pagination to src/api/users.ts` beats `worked on API`.
- Keep **Next steps** current: when a task is deferred or a new task emerges mid-session, write it down immediately.
- Do not log trivia (typo fixes, formatting) unless part of a larger change worth noting.

**Catch-up summary** (when the user returns and asks what they were working on, or asks for a project summary):

1. Read `PROJECT_LOG.md`.
2. Cross-check against the current state: `git log --oneline -20` and `git status` (if a git repo), and recently modified files.
3. Report concisely: what the project is, what was done in recent sessions, what was left unfinished, and the recorded next steps. Flag anything in the log that no longer matches reality (e.g. a "next step" that was since completed outside the log).

## File Format

```markdown
# PROJECT_LOG — <project name>

## Project overview
<What the project is, its main components, and how to run/build it. 2-5 lines.>

## Key decisions
<Decisions that outlive any single session, newest last. One line each with a date.>

## Sessions

## Session — <YYYY-MM-DD>
**Goal:** <one line>

### Changes
- <change with file path>

### Decisions & notes
- <non-obvious context worth remembering>

### Next steps
- [ ] <task>

### Status
<Done | In progress | Blocked — one line on where it stopped>
```

Notes on the format:

- Move any decision expected to outlive sessions up into **Key decisions**; a future catch-up reads that section first.
- Sessions are append-only history — never rewrite or delete past entries; supersede them with a new note.
- If the log grows past ~300 lines, fold resolved next steps and stale detail out rather than letting it bloat.

## Behavior Notes

- If the user starts a session in a project with no log, offer briefly to start one rather than silently creating files.
- The log lives in the project and may be committed; keep it free of secrets, credentials, and anything not meant for the repository.
- A git commit is a good trigger for a log update: when committing work, also update the session entry if not already current.
