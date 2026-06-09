# WordPress

I use this for WordPress and CMS work.

## Priorities

- Keep content author experience in mind.
- Shape data before it reaches presentation components when possible.
- Prefer reusable templates, components, and helpers over one-off logic.
- Keep CMS fields understandable and hard to misuse.
- Avoid putting business logic deep inside small presentational components.
- Respect the project's local WordPress, ACF, theme, and block conventions.
- Prefer `provider` over `doctor` in field names, helper names, component names, and docs copy unless quoting source content.
- Reusable clone-only ACF field groups should not be attached to real content edit screens just to keep them available. Prefer dedicated page/CPT parent field groups for real content types; clone reusable field groups into those parents and keep field names nested/prefixed to avoid top-level ACF variable collisions. Keep clone-only building block groups out of editor UIs, using the project's preferred inactive/parking pattern when needed.

## Review Lens

Check for:

- Escaping and sanitization
- Admin usability
- Field naming clarity
- Data shape consistency
- Template/component boundaries
- Reuse of existing helpers and patterns
