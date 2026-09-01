---
name: eval
description: Evaluate any PR or review comment that asks for a code change (Copilot, Claude, Mongo QA, a human, or anyone else). Use when the user says `eval`, asks to evaluate review feedback, or wants a table saying whether to implement each requested change. After eval, every inline conversation we are responsible for must be stamped (`fixed` or `Will not be fixed`) and resolved. Eval is not done while any of those threads are still open.
disable-model-invocation: true
---

# Evaluate Code Review

Use this skill when Nathan wants a decision on whether to implement requested code changes from review comments. Author does not matter: Copilot, Claude, Mongo QA, a human, or any other commenter. Our own QA comments are in scope. Leaving them open is a fail.

## Instructions

1. Collect every inline PR comment. Change-request comments go in the table. Confirm-only comments (expected behavior, no code change) stay out of the implement table, but they still get a GitHub stamp and resolve (below).
2. Read each item and the surrounding claim carefully.
3. If needed, inspect only the files needed to verify the claim.
4. Decide whether the suggestion should be implemented.
5. Output a concise table.
6. Do not implement changes unless Nathan explicitly asks.
7. When the eval is against a GitHub PR, handle the review threads (below). Do not wait for Nathan to paste or click Resolve.
8. Eval is not done while any inline conversation we are responsible for is still unresolved. Do not report eval done, or a review-fix `cp` done, with those threads still open.

## GitHub reply shape (hard rule)

Nathan reads the PR, not chat, to see the decision. After `From Mongo Grok AI:` and a blank line, the **first line of the body** is the decision. Then the explanation. Never lead with `QA:`, a finding, or a rationale.

Disagree (post at eval time, then resolve):

```
From Mongo Grok AI:

Will not be fixed. <why>
```

Agree (post only after the change is implemented and pushed, then resolve):

```
From Mongo Grok AI:

fixed
```

Confirm-only / expected behavior (no code change). Post at eval time, then resolve:

```
From Mongo Grok AI:

Will not be fixed. <expected, no code change>
```

Optional extra explanation goes **after** that decision line, never before it.

## PR review threads

Reply in the existing review thread (`POST .../pulls/{n}/comments/{comment_id}/replies`). Do not open a new top-level review body. Then resolve that conversation with GraphQL `resolveReviewThread`. GitHub replies start with `From Mongo Grok AI:` then a blank line when the user is Nathan Hiemstra.

Eval is not finished until every inline conversation we posted or are answering is stamped and resolved. That includes Mongo QA comments that led with a finding. A `QA:` comment is not a decision. Stamp it `fixed` or `Will not be fixed` on that same thread, then resolve. Do not leave GitHub conversations open for Nathan to close.

### Skip / disagree

After the eval table, reply on that existing thread using the disagree shape above. Then resolve the conversation. If there is no thread (for example a suppressed Copilot note), post an inline file comment on that line with the same shape, then resolve.

### Agree and fix

Leave the thread open at eval time. After the agreed change is implemented **and pushed**, reply using the agree shape above, then resolve the conversation.

### Confirm-only

Reply using the confirm-only shape above, then resolve. Do not skip these just because they are not in the implement table.

### Before finishing

List remaining unresolved inline threads on that PR. If any exist that we are responsible for, stamp and resolve them. Do not stop with them open.

## Output Format

Use this table:

| Comment | Suggestion | Comment |
|---|---|---|
| `<short summary of review comment>` | `agree` / `disagree` | `<blank if agreeing exactly; if agreeing with a modified approach, use: SUGGESTION: <1/2 sentence summary of original suggestion>. INSTEAD: <1/2 sentence summary of the recommended variation>.>` |

## Decision Rules

- Use `agree` when the review comment is correct and the suggested fix should be implemented as written.
- Use `agree` with a short comment when the issue is valid but the implementation should differ from the original suggestion. The comment must be self-contained and use this format: `SUGGESTION: <1/2 sentence summary of original suggestion>. INSTEAD: <1/2 sentence summary of the recommended variation>.`
- Use `disagree` when the comment is incorrect, unnecessary, out of scope, or would make the code worse.
- For `disagree`, post on the PR thread that it will not be fixed and explain why, then resolve.
- Keep comments brief and direct.
- Lead with the table. Do not add long explanation before it.

## Example

Input:

````md
- scottish-rite/templates/post-type/single.php
"get_category_link() expects a category ID; passing the full WP_Term object can trigger an object-to-int conversion error. Use $category->term_id."

- scottish-rite/components/05-pages/event/event.php
```php
<div class="border border-dotted border-primary-base p-4">
  <?php echo $args['post_info_html']; ?>
</div>
```
"This renders an empty bordered rail box whenever post_info_html is empty. It should only render the wrapper when there’s actual post info content to show."
````

Output:

| Comment | Suggestion | Comment |
|---|---|---|
| `get_category_link()` expects a category ID | agree | |
| Empty bordered rail box | agree | SUGGESTION: Only render the rail if `post_info_html` exists. INSTEAD: Also render the rail if `nice_to_haves_html` exists. |
