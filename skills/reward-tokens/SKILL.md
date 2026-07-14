---
name: reward-tokens
description: Track user-awarded response rewards in ai-foundation/tokens.md. Use when Nathan mentions giving a reward, tokens, a response pattern that worked well, or asks to update the reward/token log.
disable-model-invocation: true
---

# Reward Tokens

## Purpose

Record response patterns Nathan explicitly rewards so future agents can repeat them.

## When to Use

Use this skill when Nathan:

- says he is giving a reward
- mentions tokens
- praises a response pattern and asks to remember it
- asks to add, update, or review `/Users/nathanhiemstra/_mine/ai-foundation/tokens.md`

## Workflow

1. Read `/Users/nathanhiemstra/_mine/ai-foundation/tokens.md`.
2. Add or update a row in this format:

   `| Tokens | Reward | Agent Notes |`

3. Preserve Nathan's reward wording as closely as possible in the `Reward` column.
4. Use `Agent Notes` for a short explanation of why the rewarded pattern worked.
5. Keep entries concise. The log is a calibration aid, not a long reflection journal.

## Current Table Shape

```markdown
| Tokens | Reward | Agent Notes |
| --- | --- | --- |
```
