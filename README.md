# Product engineering template

A starting point for building a SaaS product with coding agents (Claude Code first; Codex, Grok and Gemini as builders or reviewers). It holds no product: it holds the instructions that tell an agent how to take one from an idea to production, safely.

## What it is

One coding agent doing both jobs of a small product team: **product management** (is this worth building, for whom, what to build first) and **engineering** (build it, test it, review it, ship it). The template is not code and not an agent. It is the agent's instructions, `AGENTS.md`, plus a set of **skills** it switches on.

**Skills are not agents.** A skill is a packaged method: instructions, checklists and scripts that the agent loads when a task calls for it, the way a person opens a playbook. There is still one agent, Claude Code, doing the work. Skills change *how* it works, not *who* works. The one exception is the `agent-teams` pack, which teaches the agent to hand a build or a review to *another* agent (Codex, Grok, Gemini, or a fresh Claude session), so the code is never cleared by the session that wrote it.

The skills come in six packs:

| Pack | Side | What it teaches the agent |
|---|---|---|
| `product` | PM | Research an idea and propose what to build; customer segments, the first market to win, competitors, value proposition and value curve, growth loops |
| `app-stack` | Engineering | The path from idea to production, stage by stage; the default stack (Next.js, Supabase, Clerk, Netlify); authorization in a multi-tenant SaaS; UI design; email, analytics, monitoring, legal pages, embedded agents |
| `ai-shipping` | Engineering | Tests where green means something; independent reviews; release gates; security and performance audits; docs that match the code |
| `agent-teams` | Engineering | Delegating to other agents as builders or reviewers, and checking what they report |
| `agent-harness` | Both | Writing agent instructions that get followed; hooks that make a rule impossible to break; status reports for long work; Windows shell quirks |
| `work-humanizer` | Both | Editing AI-sounding writing so a careful person could have written it |

`AGENTS.md` decides when each one applies: its § 1.8 table names, for every moment (planning a phase, fixing a bug, pushing, releasing), the rule and the skill to open.

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
