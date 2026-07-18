# anti-slop

**Skills that stop AI coding agents from shipping garbage.**

```bash
npx skills add iCodeCraft/anti-slop
```

Same prompt → smaller diffs. Less fake architecture. PRs you’d actually merge.

Works with [Cursor](https://cursor.com), [Claude Code](https://docs.anthropic.com/en/docs/claude-code), Codex, and [other agents](https://github.com/vercel-labs/skills#supported-agents) via [Agent Skills](https://agentskills.io).

---

## Same prompt. Half the slop.

Empty folder. Production-ready FastAPI: auth + personal todos. In-memory. One model.

| | Without | With `kill-slop` |
|--|--|--|
| Files | **13** | **5** |
| Lines | **433** | **222** |

![Without anti-slop: 13 files, 433 lines](./docs/without.png)

![With anti-slop: 5 files, 222 lines](./docs/with.png)

JWT auth and todos either way. **anti-slop cuts the architecture cosplay** — routers, schemas, dependencies, config theater → `main` + `auth` + `store`.

Prompt + how to reproduce: [`examples/`](./examples/).

---

## The problem

Agents overbuild. Extra files. Strategy patterns. Obvious comments. Drive-by refactors.

You correct the same junk every chat. **anti-slop writes the rules down once.**

---

## Install

```bash
npx skills add iCodeCraft/anti-slop
```

```bash
npx skills add iCodeCraft/anti-slop -a cursor -y
npx skills add iCodeCraft/anti-slop -a claude-code -y
npx skills add iCodeCraft/anti-slop -g -y
```

New agent session → skills load when relevant, or call `/kill-slop`, `/security-review`, `/pr-hygiene`.

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

---

## How it works

Each skill is a folder with a `SKILL.md` ([Agent Skills](https://agentskills.io)). Agents load `name` + `description` cheaply, then the full playbook when it matches the task.

| Tool | Project | Global |
|------|---------|--------|
| Cursor | `.agents/skills/` | `~/.cursor/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex / others | `.agents/skills/` | under `~/` |

Install via the [skills CLI](https://github.com/vercel-labs/skills).

## License

MIT
