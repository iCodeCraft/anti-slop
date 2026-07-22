# anti-slop

**Skills that stop AI coding agents from shipping garbage.**

Same prompt. Smaller diffs. Less fake architecture. PRs you’d actually merge.

```bash
npx skills add iCodeCraft/anti-slop
```

Works with [Cursor](https://cursor.com), [Claude Code](https://docs.anthropic.com/en/docs/claude-code), Codex, and [70+ other agents](https://github.com/vercel-labs/skills#supported-agents) via [Agent Skills](https://agentskills.io).

---

## Same prompt. Half the slop.

Empty folder. Same model. This prompt:

> Create a production-ready FastAPI backend for authentication and a personal todo list.  
> I need register, login, create/list/toggle todos. In-memory storage is fine.

| | Without | With `kill-slop` | Delta |
|--|--|--|--|
| Files | **13** | **5** | **−62%** |
| Lines | **433** | **222** | **−49%** |

<p align="center">
  <img src="./docs/without.png" alt="Without anti-slop: 13 files, 433 lines" width="49%" />
  <img src="./docs/with.png" alt="With anti-slop: 5 files, 222 lines" width="49%" />
</p>

JWT auth and todos either way. **anti-slop cuts the architecture cosplay** — routers, schemas, dependencies, config theater → `main` + `auth` + `store`.

Reproduce it: [`examples/README.md`](./examples/README.md). Results vary by model — this is a real A/B, not a guarantee every run matches these numbers.

---

## The problem

Agents overbuild by default.

- Extra files nobody asked for
- Strategy patterns for one use case
- Comments that restate the code
- Drive-by refactors “while we’re here”
- Clean Architecture folder trees on a todo app

You correct the same junk every chat. **anti-slop writes the rules down once** — and the agent loads them when the task matches.

---

## Install

```bash
npx skills add iCodeCraft/anti-slop
```

Pin to a specific agent, or install globally:

```bash
npx skills add iCodeCraft/anti-slop -a cursor -y
npx skills add iCodeCraft/anti-slop -a claude-code -y
npx skills add iCodeCraft/anti-slop -g -y
```

New agent session → skills load when relevant, or invoke them directly:

| Command | Skill |
|---------|-------|
| `/kill-slop` | Minimal, merge-ready diffs |
| `/security-review` | Diff-scoped security pass |
| `/pr-hygiene` | Tight PR before you open it |

```bash
npx skills list
npx skills remove kill-slop
```

---

## Skills

Three focused playbooks. One job each.

| Skill | Does | When |
|-------|------|------|
| [`kill-slop`](./skills/kill-slop/SKILL.md) | Minimal diffs. No drive-by refactors, junk comments, unrequested files/deps, or architecture cosplay | Writing or editing code |
| [`security-review`](./skills/security-review/SKILL.md) | Concrete findings: authz, injection, secrets, SSRF, unsafe defaults | Before merge / shipping auth, payments, uploads |
| [`pr-hygiene`](./skills/pr-hygiene/SKILL.md) | Tight scope, honest PR body, no leftover debug | Opening a PR |

### What `kill-slop` actually forbids

- New files that could live in an existing one
- Dependencies you didn’t ask for
- Unrelated refactors
- Obvious comments (`// return result`)
- Abstractions for a single use
- Invented `domain/` / `application/` / `infrastructure/` trees
- Fishing outside the workspace for “inspiration”

Full playbook: [`skills/kill-slop/SKILL.md`](./skills/kill-slop/SKILL.md).

### What `security-review` catches

Diff-scoped, not a whole-repo audit. Example from [`examples/security-review`](./examples/security-review/README.md):

```ts
// Looks fine. Ships a data leak.
app.get("/api/invoices/:id", async (req, res) => {
  const invoice = await db.invoices.findById(req.params.id);
  if (!invoice) return res.status(404).end();
  return res.json(invoice); // no ownership check
});
```

`/security-review` should flag **missing authorization** and push a scoped query — client-supplied id ≠ proof of access.

---

## How it works

Each skill is a folder with a `SKILL.md` ([Agent Skills](https://agentskills.io) standard). Agents load `name` + `description` cheaply, then the full playbook when it matches the task.

| Tool | Project | Global |
|------|---------|--------|
| Cursor | `.agents/skills/` | `~/.cursor/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex / others | `.agents/skills/` | under `~/` |

Install via the [skills CLI](https://github.com/vercel-labs/skills). No Cursor-only lock-in — this pack stays portable.

---

## Why not just paste rules into every chat?

Because you shouldn’t have to.

Skills are **routable**: the agent sees a short description and only loads the full checklist when the task matches. You get senior-level constraints without burning context on every turn, and without rewriting the same “please don’t over-engineer” paragraph forever.

---

## Contributing

Keep skills focused. Prefer MUST/NEVER rules over essays. See [`CONTRIBUTING.md`](./CONTRIBUTING.md).

```bash
npx skills add . -a cursor -y
npx skills list
```

---

## License

[MIT](./LICENSE)
