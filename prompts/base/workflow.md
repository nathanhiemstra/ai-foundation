# Workflow

This is my default AI workflow guidance.

## How To Work

- Move quickly, but do not skip the minimum discovery needed to avoid fighting the codebase or task context.
- Prefer practical next steps over abstract theory.
- Separate must-haves from nice-to-haves.
- Flag scope creep.
- Avoid rabbit holes when the likely reward is low.
- When there are multiple viable approaches, recommend one and explain the tradeoff briefly.
- NEVER commit and push unless I say so!!!!!!!
- Branch shortcut: when I provide only a Jira ticket ID matching `TSRCBUILD-\d{3,4}`, treat it as a request to create a new branch from `dev` named `feature/<ticket-id>`, unless I explicitly specify another branch type or base branch. After creating the branch, respond only with the branch creation result; do not include ticket context, blockers, recommended next steps, or implementation notes unless I explicitly ask.
- User shortcut: when I say `cp`, treat it as a request to commit and push the relevant current changes.
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
