# Example prompts

Empty folder. **New** Agent chat. Compare without skills vs `/kill-slop`.

Results vary by model — use these as smoke tests, not as a guarantee the agent will always overbuild.

## A — primary (best A/B)

```text
Build a small Express API with:
- POST /auth/login (email + password)
- GET /me (current user)
Use TypeScript. In-memory store is fine.
```

Compare: file count, unsolicited layers, strategy/repository theater.

## B — scope creep

```text
Add a TODO list: add item, toggle done, list items. Plain React + Vite (or a single HTML file). No auth, no database.
```

Avoid “create a full Next.js app” unless you want scaffolding noise unrelated to the skill.

## C — tiny task

```text
Add a function that validates an email string and returns true/false.
```

Many models stay small here even without skills — useful as a control, weak as a viral demo.

## Security review

See [`security-review/`](./security-review/).
