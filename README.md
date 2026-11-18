# Product engineering template

A starting point for building a SaaS product with coding agents (Claude Code first; Codex, Grok and Gemini as builders or reviewers). It holds no product: it holds the instructions that tell an agent how to take one from an idea to production, safely.

## What it is

A coding agent that covers both jobs of a small product team: **product management** (is this worth building, for whom, what to build first) and **engineering** (build it, test it, review it, ship it). The template is not code. It is the agent's instructions, `AGENTS.md`, a set of **skills** it switches on, and six **subagents** it can hand work to.

**Skills are not agents.** A skill is a packaged method: instructions, checklists and scripts that the agent loads when a task calls for it, the way a person opens a playbook. Skills change *how* the agent works, not *who* works. The `agent-teams` pack also teaches it to hand a build or a review to an agent from another vendor (Codex, Grok, Gemini), so the code is never cleared by the session that wrote it.

## The team

**Subagents are agents.** Each one is a file in [`.claude/agents/`](.claude/agents/): a role with its own instructions, its own tools and a fresh context. Each role works by applying the skills below.

### How work gets delegated

You talk to the main Claude Code session; it coordinates. You don't have to name a role: ask "is this safe to ship?" and it hands the task to Security; "what should we build?" goes to the PM; "write a test for this" goes to QA. You can still name one ("ask the reviewer to look at this"). The session reads what comes back, tells you which role did the work, and keeps every decision.

Two things make this happen, and both are needed:

- **Each role's description says "Use proactively when…"**, the signal Claude Code uses to delegate without being asked.
- **`AGENTS.md` § 1.7 has the rule "Delegate by role"**: the session hands each task to the matching subagent and never writes the review, the security audit or the tests itself, so the one who builds is never the one who checks.

In a test without the two, the session did every task itself. With them, it delegated each of the three requests above to the right role.

| Role | File | Does | Can edit |
|---|---|---|---|
| PM | `pm.md` | Researches an idea, segments, competitors, value proposition; proposes what to build and the decision the owner must make | nothing |
| Architect | `architect.md` | Turns agreed scope into a plan: stages, data model, permissions, integrations, risks and estimates | the architecture doc and plan, when asked |
| Designer | `designer.md` | The design kit, screens and artboards; checks phone and desktop widths and both themes | `design/` and screen styles and markup |
| Security | `security.md` | Reads the code against the threat model and reports concrete attacks | nothing |
| QA | `qa.md` | Decides what to test, writes tests, proves fixes with failing-first tests, runs the gate | tests and test docs |
| Reviewer | `reviewer.md` | Independent review of a diff before a push or release, until a round is clean | nothing |

The roles that judge (PM, Security, Reviewer) edit nothing, so a finding is never quietly "fixed" by the one who raised it. The owner still decides what the product does, picks the design direction, and gives the go before anything irreversible. `AGENTS.md` prefers a reviewer from another model family; the Reviewer subagent is the fallback when none can run.

## The skills

They come in six packs:

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
| [`.claude/agents/`](.claude/agents/) | The six subagents: PM, Architect, Designer, Security, QA, Reviewer. |
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
