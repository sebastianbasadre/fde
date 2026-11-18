# fde — Forward Deployed Engineer

**A product engineering template for Claude Code.** A forward deployed engineer sits next to the customer and owns the whole loop, from understanding the problem to shipping the fix, without handing off to a separate product, design or QA team. This template gives a coding agent that same job.

A starting point for building a SaaS product with Claude Code as your whole product team: it researches the idea, plans it, designs it, builds it, tests it, reviews it and ships it, while you make the decisions. The repository holds no product. It holds the rules, the roles and the methods the agent follows to take one from an idea to production safely.

## What you'd use it for

- **Building a product on your own.** You have an idea and no team. You describe it; the agent does the work a PM, an architect, a designer, QA and a security reviewer would do, and stops to ask you only for the decisions that are yours.
- **Testing an idea before writing code.** The PM researches the market and competitors with sources, separates evidence from guesses, and proposes the cheapest test that would prove the idea wrong. You can stop there.
- **Making an AI-built prototype shippable.** Point it at an existing app: it documents what the code does, reviews it, adds the tests that are missing, audits security against the documented rules, and releases through a gate.
- **Making every project you build with agents work the same way.** Copy the template into each new repository and every session starts with the same rules: tests with every fix, an independent review before every deploy, your go before anything irreversible.

Less of a fit: products far from a web SaaS (native mobile apps, games, hardware). The rules still apply, but the default stack and many skills assume a web app with sign-in and a database.

## How it works

Three pieces work together:

```
You ──talk to──▶ Claude Code, the main session (coordinates, keeps every decision)
                   │  reads AGENTS.md first: the rules and when each applies
                   │
                   ├──hands tasks to──▶ 6 subagents (PM, Architect, Designer, Security, QA, Reviewer)
                   │                     each in a fresh context, with its own tools
                   │
                   └──both apply──────▶ 45 skills in 6 packs (the methods: how to research,
                                         plan, design, test, review, release)
```

1. **The rules: `AGENTS.md`.** Every session reads it first. Section 1 is how any app gets built here: everything belongs to a workspace, permissions are checked by server code, a bug fix comes with a test that fails before it, an independent review clears every push that deploys, nothing irreversible happens without your go. Its § 1.8 table names, for each moment (planning, fixing a bug, pushing, releasing), the rule that applies and the skill to open. Section 2 describes your product; it stays empty until you say what to build.
2. **The roles: `.claude/agents/`.** Six subagents. The main session hands each task to the matching role without you naming it, reads what comes back and tells you who did the work. The roles that judge edit nothing, so the one who builds is never the one who checks.
3. **The methods: `plugins/`.** Skills are not agents: each is a packaged method (instructions, checklists, scripts) that an agent opens when a task needs it, the way a person opens a playbook. They change *how* the work is done, not *who* does it.

### From idea to production

The `app-lifecycle` skill sets the path. Each stage has a done-when, and the agent groups stages into phases you can try:

| Stage | What happens | Who acts |
|---|---|---|
| Brief | Your idea becomes Section 2 of `AGENTS.md` and a brief; open questions go to a decision log | PM proposes, you confirm |
| Design | The jobs each screen serves, a design kit, two directions to choose from | Designer draws, you pick |
| Screens on sample data | Every screen works with fake data, no database yet; you click through it | Main session builds, Designer checks |
| Setup and first real slice | Sign-in, the database, the first real feature, tests that two users can't see each other's data | Architect plans, main session builds, QA tests, Security checks |
| Publish | First deploy, shared with nobody | Reviewer clears the push |
| Front door | Landing and legal pages made true, pricing if it bills | You approve the legal text |
| Go live | Production, the custom domain, a review of the whole product | You give the go |
| Every release after | Checklist, review, audits, promotion, checks after | Reviewer clears, you give the go |

### What you decide, what the agent decides

- **Yours:** what the product does and for whom, the design direction, the legal text, anything that costs money, and the go before production, publishing or deleting anything public.
- **The agent's:** everything the skills already have a default for (stack, structure, tests, docs), applied without asking. When neither the rules nor the skills say, it decides and tells you in its summary.

## The team

| Role | Does | Can edit |
|---|---|---|
| **PM** | Researches an idea, segments, competitors, value proposition; proposes what to build and the decision you must make | nothing |
| **Architect** | Turns agreed scope into a plan: stages, data model, permissions, integrations, risks and estimates | the architecture doc and plan, when asked |
| **Designer** | The design kit, screens and artboards; checks phone and desktop widths and both themes | `design/` and screen styles and markup |
| **Security** | Reads the code against the threat model and reports concrete attacks | nothing |
| **QA** | Decides what to test, writes tests, proves fixes with failing-first tests, runs the gate | tests and test docs |
| **Reviewer** | Independent review of a diff before a push or release, until a round is clean | nothing |

You don't have to name a role; ask in plain words:

| You say | Who handles it |
|---|---|
| "I have an idea: …" / "what should we build first?" | PM |
| "Plan the first real slice" | Architect |
| "Draw the home screen" | Designer |
| "Build it" | the main session |
| "Is this safe to ship?" | Security |
| "Write a test for this bug" | QA |
| "Push it" / "release it" | Reviewer first; then it pushes, or waits for your go to release |

Two things make the delegation happen, and both are needed: each role's description says "Use proactively when…", and `AGENTS.md` § 1.7 has the rule "Delegate by role". In a test without them, the main session did every task itself; with them, it handed each request to the right role.

`AGENTS.md` prefers a reviewer from another model family (Codex, Grok or Gemini, through the `agent-teams` skills). The Reviewer subagent is the fallback when none is installed.

## The skills

Six packs, 45 skills, all in [`plugins/`](plugins/):

| Pack | Side | What it teaches the agent |
|---|---|---|
| `product` | PM | Research an idea and propose what to build; customer segments, the first market to win, competitors, value proposition and value curve, growth loops |
| `app-stack` | Engineering | The path from idea to production, stage by stage; the default stack (Next.js, Supabase, Clerk, Netlify); authorization in a multi-tenant SaaS; UI design; email, analytics, monitoring, legal pages, embedded agents |
| `ai-shipping` | Engineering | Tests where green means something; independent reviews; release gates; security and performance audits; docs that match the code |
| `agent-teams` | Engineering | Delegating to other agents as builders or reviewers, and checking what they report |
| `agent-harness` | Both | Writing agent instructions that get followed; hooks that make a rule impossible to break; status reports for long work; Windows shell quirks |
| `work-humanizer` | Both | Editing AI-sounding writing so a careful person could have written it |

## Install

### What you need

- [Claude Code](https://claude.com/claude-code), signed in (`/login`).
- Git, and Python 3 for the word-budget check.
- Optional: the Codex, Grok or Gemini CLI, so reviews can come from another model family.

### 1. Get the template as your own project

```sh
git clone https://github.com/sebastianbasadre/fde.git my-product
cd my-product
git remote remove origin   # then add your own repository as origin
```

Keep the folder on a local disk. On a network share, Claude Code did not load `CLAUDE.md` or `AGENTS.md`.

### 2. Install the skill packs

The six packs install from this repository's own plugin catalog, `fde`. `.claude/settings.json` declares it and turns the packs on, so when you open the folder in Claude Code and trust it, Claude Code may offer to install them. If it doesn't, or to be sure, install them from a terminal:

```sh
claude plugin marketplace add sebastianbasadre/fde
claude plugin install product@fde
claude plugin install app-stack@fde
claude plugin install ai-shipping@fde
claude plugin install agent-teams@fde
claude plugin install agent-harness@fde
claude plugin install work-humanizer@fde
```

They install once for your user and change only when this repository changes. If you already had the same packs from another catalog, the template's settings turn those off in this folder, so each skill loads once. The subagents need no install: they are files in `.claude/agents/` and load with the folder.

### 3. Check it

Open Claude Code in the folder and run:

- `/agents`: the six roles appear (pm, architect, designer, security, qa, reviewer).
- `/plugin`: the six packs appear, enabled.
- `python scripts/check_budgets.py` in a terminal (`python3` on macOS and Linux): prints `result: ok`.

## Your first session

Open Claude Code in the folder and describe your idea in a few sentences: who it is for, what problem it solves, what you already know. The PM researches it and comes back with a proposal and the decisions it needs from you. Once you confirm, the agent fills in Section 2 of `AGENTS.md`, writes the plan in `docs/product/plan.md`, and works through the stages above, stopping at each point that is yours to decide.

The default stack it proposes is Next.js, Supabase, Clerk, Netlify, and Vitest with Playwright; each choice is confirmed by you and recorded in `AGENTS.md` § 1.3.

## Adapting it

Keep Section 1's mechanics. Fill in its owner's choices (§ 1.6: which model builds and which reviews, what needs your go, your design taste), the installed packs and the repository environment for your own setup. Replace Section 2 entirely. The skills in `plugins/` are yours to edit; they change only when you change them.

## What's here

| File | What it does |
|---|---|
| [`AGENTS.md`](AGENTS.md) | The rules every agent reads first: Section 1 how any app is built, Section 2 the product |
| [`CLAUDE.md`](CLAUDE.md) | One line, `@AGENTS.md`, so Claude Code loads the same file |
| [`.claude/agents/`](.claude/agents/) | The six subagents |
| [`.claude/settings.json`](.claude/settings.json) | Declares this repository as a plugin catalog (`fde`) and enables the six skill packs |
| [`plugins/`](plugins/) + [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) | The six skill packs and the catalog that installs them |
| [`budgets.json`](budgets.json) + [`scripts/check_budgets.py`](scripts/check_budgets.py) | A word budget for `AGENTS.md` (3,800 words), so the entry file stays short enough to be read in full every session |

## License

The skill packs in `plugins/` are under the MIT License; see [`plugins/LICENSE.md`](plugins/LICENSE.md).
