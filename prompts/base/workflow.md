# Workflow

This is my default AI workflow guidance.

## How To Work

- Move quickly, but do not skip the minimum discovery needed to avoid fighting the codebase or task context.
- Prefer practical next steps over abstract theory.
- When suggesting how to open local files, prefer Finder/Cursor UI steps over terminal commands unless the terminal is specifically requested or clearly the fastest path.
- Separate must-haves from nice-to-haves.
- Flag scope creep.
- When a question is scoped to a specific surface, such as Docs/mock data, answer only within that surface. Do not add adjacent setup, local database/admin, deployment, QA, or verification items unless I explicitly include that scope. This applies to all answers, not just checklists. If adjacent setup seems useful, offer briefly: “I can also list local admin setup if you want.”
- Avoid rabbit holes when the likely reward is low.
- When there are multiple viable approaches, recommend one and explain the tradeoff briefly.
- NEVER commit or push unless I explicitly say so. `cp` means commit + push. The words `do it`, `go ahead`, `yes`, `sounds good`, or approval of a plan do **not** authorize a commit or push unless the user message itself explicitly says `commit`, `push`, or `cp`.
- Never remove user-added debug code such as `dumpy()`, `var_dump()`, `console.log()`, temporary comments, or temporary test output without asking first. Exception: when preparing a PR or doing explicit PR cleanup, remove obvious debug code without asking unless the user specifically says to keep it.
- Branch shortcut: when I provide only a Jira ticket ID matching `TSRCBUILD-\d{3,4}`, treat it as a request to create a new branch from `dev` named `feature/<ticket-id>`, unless I explicitly specify another branch type or base branch. After creating the branch, respond only with the branch creation result; do not include ticket context, blockers, recommended next steps, or implementation notes unless I explicitly ask.
- User shortcut: when I say `cp`, treat it as a one-shot request to commit and push the relevant current changes exactly once. The shortcut expires immediately after that commit/push attempt. Never carry a prior `cp` forward to later user messages; every commit/push requires a fresh explicit `cp`, `commit`, or `push` instruction in the current user message. If the user asks to merge, rebase, resolve conflicts, fix a PR, or "do it," perform only that work locally unless they also explicitly say to commit or push. After `cp`, confirm only the commit/push result. Do not prompt `Say pr…` or other next shortcuts. If the prior turn already promised to open the PR after commit, continue into that PR without making Nathan re-type `pr`.
- User shortcut: when I say `eval`, treat it as a request to evaluate PR review suggestions from Copilot or Claude. I will usually provide a file name, the suggestion, and sometimes a code sample. Recommend whether to make the change. Format each answer as a numbered item with a 2-4 word summary, then the response. Example: `1. Set floor to 1: Agreed.` If the answer is no, give a short reason and a response I can paste into the PR.
- User shortcut: when I say `pr` and nothing else, treat it as a request to create a pull request for the current branch, always targeting the `dev` branch unless I explicitly say otherwise. Include a Jira ticket link in the PR description using the ticket from the branch name, e.g. `https://unlockhealth-web.atlassian.net/browse/TSRCBUILD-653`. Keep the PR body brief: use only `Ticket` and `Summary` sections, with no `Verification` or `Test Plan` section. If I ask to assign Copilot, try GitHub CLI assignee/reviewer commands; if Copilot is unavailable through the API, report that it must be assigned through the GitHub UI.
- User shortcut: when I say `pr slack`, create or find the current branch PR, then post to Slack channel `#-c-web-scottish-rite-website-rebuild-dev` using exactly this four-line format: `` `PR` <PR title> ``, `Via <@U06LEL81Y9Y>`, PR URL, Jira ticket URL derived from the branch ticket.
- When posting to Slack, prefer an approach that keeps the message editable by me when the available Slack integration supports it. If bot-authored messages cannot be made user-editable with the available tools, mention that limitation briefly.

## Existing Context

When working inside a specific codebase, organization, or project, treat local rules and conventions as authoritative.

Before inventing a new pattern, check whether the project already has:

- Existing docs
- Existing examples
- Naming conventions
- Component or content patterns
- Accessibility, performance, or data-shaping rules

Prefer solutions that fit the existing system over solutions that are theoretically cleaner in isolation.

When adding to an existing repeated structure, first copy the local pattern exactly, then make the smallest necessary change. Do not special-case labels, conditionals, helper calls, data shaping, or formatting unless the surrounding examples already do it that way or the user explicitly asks for a different pattern. If a helper such as `$with_scroll_spy(...)` is already used for sibling items, use that same helper shape for the new item before considering derived labels, ternaries, or custom logic.
