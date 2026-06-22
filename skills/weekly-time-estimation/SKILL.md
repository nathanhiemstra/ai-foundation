---
name: weekly-time-estimation
description: Estimate how much time the user worked on tasks during the previous week for timesheet entry. Use when the user asks to estimate last week's time, reconstruct work hours, prepare a timesheet, enter time, or review weekly work across Git, Jira, Slack, calendar, terminal history, and local transcripts.
---

# Weekly Time Estimation

## Defaults

- Scope: previous Sunday-Saturday in the user's local time unless they specify different dates.
- Evidence: use ActivityWatch first when available, then Git commits/branches, PRs, Jira, Slack, calendar context, terminal history, local files, and agent transcripts.
- Output: start with a per-day time-entry table for Adobe Experience Cloud, then summarize by ticket/task with confidence and evidence notes. Always call out meetings/admin separately.
- Rounding: nearest `0.25h`.
- Be practical: estimates are acceptable, but label confidence and call out gaps.
- Do not enter time or modify external systems unless the user explicitly asks.

## Workflow

1. Confirm the date range if it is ambiguous.
2. Gather evidence for the range:
   - ActivityWatch: app/window/browser activity, idle gaps, and project-specific page titles.
   - Git: commits, branches, PRs, changed files.
   - Jira: assigned/in-progress/done issues, comments, transitions, worklogs if available.
   - Slack: task handoffs, PR announcements, support requests, coordination.
   - Calendar or meeting notes if available.
   - Local terminal history and agent transcripts when relevant.
3. Group evidence into task buckets:
   - Prefer Jira ticket IDs when available.
   - Use a short task label when no ticket exists.
   - Split unrelated work even if it happened in one repo/session.
   - Keep meetings/admin as a separate bucket unless a meeting is clearly tied to one specific ticket.
4. Estimate time:
   - Use direct evidence first: meeting duration, commit/PR timestamps, explicit notes.
   - Use activity spans cautiously; subtract obvious idle gaps.
   - For context switching, assign small blocks to the most likely task and mark confidence lower.
   - Round final task totals to the nearest `0.25h`.
5. Reconcile:
   - Check whether the total looks plausible against the user's normal workday/week.
   - Flag unallocated or uncertain time instead of forcing exact precision.
   - Make sure each day's ticket/task rows plus meetings/admin rows add up cleanly.
6. Ask follow-up questions only when they would materially change totals.

## ActivityWatch

When ActivityWatch is installed:

1. Check `http://localhost:5600` or the local API at `http://localhost:5600/api/0/buckets`.
2. Use ActivityWatch as the primary timeline for active computer time, idle gaps, app names, window titles, and browser tabs.
3. Group ActivityWatch events by likely task:
   - Cursor/terminal/repo window titles -> coding task or repo.
   - Jira/GitHub URLs and titles -> ticket or PR.
   - Slack titles -> coordination/support time.
   - Browser docs/search pages -> research for the nearest active task.
4. Cross-check ActivityWatch spans against Git/Jira/Slack evidence before assigning hours.
5. Do not count idle time. Treat long ambiguous browser/app spans as `Low` or `Medium` confidence unless other evidence supports them.
6. If the local API is unavailable, ask the user to export ActivityWatch data or open the ActivityWatch dashboard and provide the relevant summary.

## Confidence Labels

- `High`: direct evidence from calendar/worklog/clear commit or PR activity.
- `Medium`: good artifact trail, but exact duration inferred.
- `Low`: weak evidence, fragmented context, or likely missing artifacts.

## Output Format

Use this default format for Adobe Experience Cloud time entry:

```markdown
## Daily Time Entry

| Date | Day | Ticket/Task | Hours | Description | Confidence |
| --- | --- | --- | ---: | --- | --- |
| YYYY-MM-DD | Mon | TSRCBUILD-123 | 1.25 | Short plain-English work description. | Medium |
| YYYY-MM-DD | Mon | Meetings/Admin | 0.50 | Project status meeting. | High |

## Meetings / Admin

| Date | Meeting/Admin Bucket | Hours | Notes | Confidence |
| --- | --- | ---: | --- | --- |
| YYYY-MM-DD | Project/status meetings | 1.00h | Calendar-backed. | High |

## Ticket/Task Rollup

| Ticket/Task | Estimate | Confidence | Notes |
| --- | ---: | --- | --- |
| TSRCBUILD-123 - Short label | 2.25h | Medium | PR + commits across Mon/Tue; exact start/stop inferred. |

## Gaps / Questions

- [Only list gaps that affect the estimate.]

## Total

Estimated total: `0.00h`
Task work: `0.00h`
Meetings/admin: `0.00h`
```

## Description Style

- Keep descriptions short and timesheet-safe.
- Use plain work descriptions: "Reviewed PR feedback and updated Relevanssi ACF indexing rules."
- Avoid over-detailed evidence in timesheet rows.
- Keep internal uncertainty in the notes section, not in the time-entry description.
