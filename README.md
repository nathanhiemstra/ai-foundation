# ai-foundation

My AI rules and reusable prompts.

## How This Loads Into AI Tools

**Claude Code (automatic):** `CLAUDE.md` at the repo root is a condensed version of everything here. Symlink it to `~/.claude/CLAUDE.md` so it loads globally in every Claude Code session, regardless of which repo you're in:

```bash
ln -s ~/ai-foundation/CLAUDE.md ~/.claude/CLAUDE.md
```

Update the repo → Claude Code picks it up automatically.

**Cowork (manual):** Cowork custom instructions aren't file-based — there's no auto-sync. Keep the source of truth here and re-paste into Settings when it changes. Two places to update:
- **Settings > Custom Instructions** → communication + workflow preferences (global, all projects)
- **Settings > Projects > [project] > Instructions** → project-specific context only

**Cursor / other tools:** Load prompts manually per session from `prompts/`.

## Structure

- `CLAUDE.md` - condensed always-on context for Claude Code (symlink to `~/.claude/CLAUDE.md`)
- `prompts/base/` - general communication and workflow preferences
- `prompts/roles/` - role-specific context
- `prompts/domains/` - reusable domain guidance
- `prompts/workflows/` - task-specific workflow prompts
- `skills/` - portable skill workflows to mirror into tools like Cursor and Cowork
- `memory/` - reusable decisions and recurring patterns

## Prompts

Start with the base prompts, then add the role/domain/workflow prompts needed for the task.

### Base

- [Communication](prompts/base/communication.md)
- [Workflow](prompts/base/workflow.md)

### Roles

- [Senior Front-End Developer](prompts/roles/senior-front-end-developer.md)

### Domains

- [Accessibility](prompts/domains/accessibility.md)
- [Front-End](prompts/domains/frontend.md)
- [WordPress](prompts/domains/wordpress.md)

### Workflows

- [Code Review](prompts/workflows/code-review.md)
- [Component Planning](prompts/workflows/component-planning.md)
- [Ticket Cleanup](prompts/workflows/ticket-cleanup.md)

### Skills

- [Weekly Time Estimation](skills/weekly-time-estimation/SKILL.md)
- [The Humanizer](skills/the-humanizer/SKILL.md)
