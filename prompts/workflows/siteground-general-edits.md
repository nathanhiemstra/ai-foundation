# SiteGround General Edits

Use this workflow when making general edits to a SiteGround-hosted WordPress site, especially when the change is more than a very small production-safe fix.

## Goal

Make edits through a SiteGround staging copy first, review them safely, then merge to production only after approval.

## Steps

1. Go to SiteGround Staging Copies:

```txt
https://tools.siteground.com/wp-staging
```

2. Create a staging copy.

Use a clear name that identifies the work, for example:

```txt
campfest2026.vbyc.org
```

3. Open the staging site admin.

In the staging copy row, click:

```txt
Actions → Log Into Admin Panel
```

4. Confirm the staging environment before editing.

- Verify the browser URL is the staging domain, not production.
- Confirm the WordPress admin bar/site title also indicates staging when possible.
- If using SSH/SFTP, verify the remote path is the staging root, not production.

5. Choose the editing path.

- For simple content/editor updates, make edits directly in WordPress Admin on staging.
- For simple code changes, work with AI against the staging site/files when the change is clear and low-risk.
- For complex code changes, pull a copy of the staging site down locally before editing. Use the local copy for development, testing, and review, then push changes back to staging.
- If the scope is unclear, treat it as complex until the affected files, data, and review path are understood.

6. Make the requested edits on staging or the local staging copy.

- Prefer normal WordPress admin changes for content/editor-owned updates.
- Use SSH/SFTP or file edits only when the change belongs in code.
- Keep Sass and compiled CSS in sync for theme styling changes.
- Do not edit WordPress core.
- Treat schedules, Stripe, Donorbox, donations, checkout, API keys, webhooks, and donor data as high-risk.

7. Review the staging site.

- Check the changed pages on desktop and mobile widths.
- Check keyboard/focus behavior for interactive changes.
- Verify admin/editor behavior if content fields or templates changed.
- For schedule/payment/donation work, do extra focused review before approval.

8. Ask for approval before production merge.

Do not merge staging to production, deploy files, sync databases, or overwrite production until the user explicitly approves.

9. Merge back to production only after approval.

Use SiteGround's staging merge tools when possible. If manual file/database work is needed, confirm the exact plan first and make a backup before any production sync.

## Safety

- Prefer staging for anything beyond a very small/simple change.
- Do not run deploy, sync, rsync, database import/export, or production overwrite commands without explicit approval.
- Before remote database/content sync, make a remote backup first.
- Keep production and staging paths/domains visibly distinct in notes and commands.
- If SSH is needed before editing, follow `siteground-ssh.md` first.
