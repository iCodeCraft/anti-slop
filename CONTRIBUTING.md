# Contributing

1. Keep each skill focused — one job per `SKILL.md`.
2. Descriptions must include **what** and **when** (routing keywords).
3. Prefer MUST/NEVER rules and checklists over essays.
4. Add or update prompts under `examples/` when behavior changes.
5. Don't add Cursor-only `.cursor/rules` — this pack stays portable.

Validate locally:

```bash
npx skills add . -a cursor -y
npx skills list
```
