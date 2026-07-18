# anti-slop

**Skills that stop AI coding agents from shipping garbage.**

```bash
npx skills add iCodeCraft/anti-slop
```

Drop-in [Agent Skills](https://agentskills.io) for Cursor, Claude Code, Codex, and [other agents](https://github.com/vercel-labs/skills#supported-agents).

Same prompt → smaller diffs, less fake architecture, PRs you’d actually merge.

---

## The problem

Coding agents are smart, but they overbuild: extra files, strategy patterns, obvious comments, drive-by refactors. You repeat the same corrections every chat. **anti-slop writes them down once.**

---

## Install

```bash
npx skills add iCodeCraft/anti-slop
```

Optional targeting:

```bash
npx skills add iCodeCraft/anti-slop -a cursor -y
npx skills add iCodeCraft/anti-slop -a claude-code -y
npx skills add iCodeCraft/anti-slop -g -y
```

New agent session → `/kill-slop`, `/security-review`, or `/pr-hygiene`.

```bash
npx skills list
npx skills remove kill-slop
```

---

## Skills

| Skill | Does |
|-------|------|
| [`kill-slop`](./skills/kill-slop/SKILL.md) | Minimal diffs. No drive-by refactors, junk comments, or unrequested files/deps |
| [`security-review`](./skills/security-review/SKILL.md) | Diff review: authz, injection, secrets, SSRF, unsafe defaults |
| [`pr-hygiene`](./skills/pr-hygiene/SKILL.md) | Tight scope, honest PR body, no leftover debug |

Try prompts on an empty project: [`examples/`](./examples/).

---

## How it works

Skills are folders with a `SKILL.md` ([Agent Skills](https://agentskills.io) format). Agents load `name` + `description` cheaply, then the full playbook when relevant.

| Tool | Project | Global |
|------|---------|--------|
| Cursor | `.agents/skills/` | `~/.cursor/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex / others | `.agents/skills/` | under `~/` |

Install via the [skills CLI](https://github.com/vercel-labs/skills).

## License

MIT
