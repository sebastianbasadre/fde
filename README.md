# Product engineering template

A starting point for building a SaaS product with coding agents (Claude Code first; Codex, Grok and Gemini as builders or reviewers). It holds no product: it holds the instructions that tell an agent how to take one from an idea to production, safely.

## What's here

| File | What it does |
|---|---|
| [`AGENTS.md`](AGENTS.md) | The instructions every agent reads first. **Section 1** is how we build any app: ownership and authorization, threat model, stack, testing and delivery, working rules, and a table of what to open at each moment. **Section 2** is the product, left empty until the owner says what to build. |
| [`CLAUDE.md`](CLAUDE.md) | One line, `@AGENTS.md`, so Claude Code loads the same file. |
| [`.claude/settings.json`](.claude/settings.json) | Enables the skill packs `AGENTS.md` relies on: `product`, `ai-shipping`, `agent-teams`, `agent-harness`, `app-stack`, `work-humanizer`. |
| [`budgets.json`](budgets.json) + [`scripts/check_budgets.py`](scripts/check_budgets.py) | A word budget for `AGENTS.md` (3,800 words), checked in the test gate, so the entry file stays short enough to be read in full every session. |

## Starting a product from it

1. Copy the repository and open it in Claude Code (from a local folder, not a network share: on one, Claude Code did not load `CLAUDE.md` or `AGENTS.md`).
2. Describe your idea. The agent runs the `product:propose` skill, then fills in Section 2 of `AGENTS.md` at the brief stage, following the `app-lifecycle` skill.
3. From there, each phase follows the plan the agent writes in `docs/product/plan.md`, with an independent review before every push that deploys and the owner's go before anything irreversible.

The default stack the agent proposes is Next.js, Supabase, Clerk, Netlify, and Vitest with Playwright; each choice is confirmed by the owner and recorded in § 1.3.

## Adapting it

Keep Section 1's mechanics; fill in its owner's choices (§ 1.6), the installed packs and the repository environment for your own setup. Replace Section 2 entirely.

## Checks

```sh
python scripts/check_budgets.py
```

Exits 0 when `AGENTS.md` is within its budget.
