# gstack Skills Guide — Layman's Reference

> Quick-reference for all gstack slash commands, grouped by workflow phase.
> Use this offline to know what to run and when.

---

## How It Works

Every skill runs a **preamble** first — a shared bootstrap that detects your branch,
repo mode, whether to proactively suggest other skills, and sets up the session.
Some skills have a `BENEFITS_FROM` relationship (e.g., plan reviews benefit from
`/office-hours` having been run first). When a prerequisite hasn't been run, the
skill will offer to run it inline before continuing.

---

## Phase 0 — Ideation & Strategy

*Before you write any code or even a plan. "What are we building and why?"*

| Command | What It Does |
|---------|-------------|
| `/office-hours` | YC-style brainstorming session. Two modes: **Startup** (6 forcing questions to stress-test your idea) and **Builder** (design thinking for a specific feature). Saves a design doc. |
| `/design-consultation` | Creates a full design system from scratch — aesthetic direction, typography, color palette, spacing, motion. Generates preview pages and a `DESIGN.md`. Use when starting a new project with no visual identity yet. |

**When to use:** Starting something new, pivoting, or questioning "should we even build this?"

---

## Phase 1 — Plan Review

*You have an idea or plan. Now stress-test it from three angles before writing code.*

| Command | What It Does |
|---------|-------------|
| `/plan-ceo-review` | **CEO/founder lens.** Challenges your premises, asks "what's the 10-star version?", suggests scope changes. Four modes: expand scope, selectively expand, hold scope, reduce scope. Benefits from `/office-hours`. |
| `/plan-design-review` | **Designer lens.** Rates your plan on design dimensions (0-10), explains what a 10 looks like, then rewrites the plan to hit it. For UI/UX-heavy features. Works on the *plan*, not live code. |
| `/plan-eng-review` | **Eng manager lens.** Locks in architecture, data flow, edge cases, test strategy, performance considerations. The "how do we actually build this?" review. Benefits from `/office-hours`. |
| `/autoplan` | **All three in one command.** Runs CEO → design → eng review sequentially with auto-decisions. Surfaces only the "taste decisions" (genuinely ambiguous calls) for your input. Use when you want the full pipeline without running each manually. |

**When to use:** After you have a rough plan, before you start coding. Run all three (or `/autoplan`) for maximum rigor.

**Order:** `/office-hours` → `/plan-ceo-review` → `/plan-design-review` → `/plan-eng-review` (or just `/autoplan` for all three)

---

## Phase 2 — Implementation

*You're writing code. These are utility skills that help during development.*

| Command | What It Does |
|---------|-------------|
| `/browse` | Headless browser — navigate pages, click elements, take screenshots, test responsive layouts, run JS. Used by many other skills under the hood (`$B`). |
| `/connect-chrome` | Launches a real Chrome browser controlled by gstack with the Side Panel extension auto-loaded. Lets you watch everything gstack does in the browser in real time. |
| `/investigate` | Systematic root-cause debugging. Four phases: investigate → analyze → hypothesize → implement. **Iron law: no fixes without understanding the root cause first.** Use when something is broken. |
| `/codex` | Second opinion from OpenAI's Codex CLI. Three modes: **Review** (independent diff review), **Challenge** (adversarial "try to break it"), **Consult** (ask anything). The "200 IQ second opinion." |
| `/learn` | Manages project learnings across sessions — review, search, prune, and export what gstack has discovered. Keeps track of patterns and past solutions so knowledge persists. |

**When to use:** During active development whenever you need to test, debug, or get a second opinion.

---

## Phase 3 — QA & Visual Polish

*Code is written. Does it actually work? Does it look right?*

| Command | What It Does |
|---------|-------------|
| `/qa` | **Test and fix.** Systematically tests your web app, finds bugs, then fixes them with atomic commits and re-verification. Produces before/after health scores. Three tiers: Quick (critical only), Standard (+ medium), Exhaustive (+ cosmetic). |
| `/qa-only` | **Test and report only.** Same systematic testing as `/qa` but produces a bug report without touching any code. Use when you want to see what's broken without auto-fixing. |
| `/design-review` | **Visual polish on live site.** Designer's eye for spacing, hierarchy, inconsistency, "AI slop" patterns, slow interactions. Fixes issues iteratively with atomic commits. This works on the *running app*, not the plan. |
| `/design-shotgun` | **Multi-variant design exploration.** Generates multiple AI design variants, opens them side-by-side for comparison, and collects structured feedback. Great for exploring visual directions quickly. |
| `/design-html` | **Mockup → production HTML.** Takes an approved AI mockup and generates production-quality HTML/CSS where text reflows properly and heights are dynamic. Use after a design is approved. |
| `/benchmark` | **Performance regression detection.** Measures page load, Core Web Vitals, bundle sizes. Compares before/after so you catch perf regressions before shipping. |

**When to use:** After your feature is working locally, before shipping.

**Order:** `/qa` → `/design-review` → `/benchmark`

> **Design skill clarification:**
> - `/design-consultation` = create a design system (Phase 0, new projects)
> - `/plan-design-review` = review a *plan* for design quality (Phase 1, before coding)
> - `/design-shotgun` = generate multiple design variants side-by-side to pick a direction (Phase 3)
> - `/design-review` = review a *live site* for visual issues and fix them (Phase 3, after coding)
> - `/design-html` = convert an approved mockup into production HTML/CSS (Phase 3, after approval)

---

## Phase 4 — Code Review & Ship

*Everything works and looks good. Time to review, version, and create the PR.*

| Command | What It Does |
|---------|-------------|
| `/review` | **Pre-landing code review.** Analyzes your diff for SQL safety, LLM trust boundary violations, conditional side effects, structural issues. Two-pass: CRITICAL findings first, then INFORMATIONAL. |
| `/cso` | **Security audit.** Chief Security Officer mode — secrets archaeology, dependency supply chain, CI/CD pipeline security, LLM/AI security, OWASP Top 10, STRIDE threat modeling. Two modes: **daily** (fast, 8/10 confidence bar) and **comprehensive** (deep monthly scan, 2/10 bar). |
| `/ship` | **Automated ship workflow.** Detects base branch, merges, runs tests, reviews diff, bumps VERSION, updates CHANGELOG, commits, pushes, creates PR. Fully automated unless it hits a gate condition. |

**When to use:** Feature is QA'd and polished. Ship it.

**Order:** `/review` → `/cso` (optional, for security-sensitive changes) → `/ship`

---

## Phase 5 — Deploy & Verify

*PR is created. Merge it, deploy it, make sure production is healthy.*

| Command | What It Does |
|---------|-------------|
| `/land-and-deploy` | **Merge → deploy → verify.** Merges your PR, waits for CI and deploy, runs canary health checks on production. The bridge between "PR approved" and "safely in production." |
| `/canary` | **Post-deploy monitoring.** Watches the live app for console errors, performance regressions, page failures. Takes periodic screenshots and compares against baselines. Use for ongoing monitoring after deploy. |

**When to use:** After `/ship` creates the PR and it's approved.

**Order:** `/land-and-deploy` → `/canary` (for ongoing monitoring)

---

## Phase 6 — Documentation & Reflection

*It's live. Update the docs and reflect on how it went.*

| Command | What It Does |
|---------|-------------|
| `/document-release` | **Post-ship doc update.** Reads all docs, cross-references the diff, updates README/ARCHITECTURE/CONTRIBUTING/CLAUDE.md to match what shipped. Polishes CHANGELOG voice, cleans TODOs. |
| `/retro` | **Engineering retrospective.** Analyzes commit history, work patterns, code quality. Supports 24h / 7d / 14d / 30d windows, team-aware breakdowns with per-person praise and growth areas. Also has a **global** mode for cross-project retros. |

**When to use:** After shipping. `/document-release` right away, `/retro` at end of week/sprint.

---

## Safety & Guardrails (use anytime)

*These restrict what Claude can do. Use when working near production or sensitive systems.*

| Command | What It Does |
|---------|-------------|
| `/careful` | Warns before destructive commands (rm -rf, DROP TABLE, force-push, etc.). |
| `/freeze` | Restricts file edits to a specific directory. Blocks writes outside that path. |
| `/guard` | **Maximum safety.** Combines `/careful` + `/freeze`. Use when touching prod. |
| `/unfreeze` | Removes the `/freeze` restriction, allowing edits everywhere again. |

---

## Setup & Maintenance (one-time or occasional)

| Command | What It Does |
|---------|-------------|
| `/setup-deploy` | **One-time.** Configures deployment settings — detects your platform (Fly, Render, Vercel, Netlify, etc.), production URL, health checks. Writes config to CLAUDE.md so `/land-and-deploy` knows how to deploy. |
| `/setup-browser-cookies` | **One-time.** Imports cookies from your real browser (Chrome, Arc, Brave, Edge) into the headless browse session so it can test authenticated pages. |
| `/gstack-upgrade` | Updates gstack to the latest version. |

---

## The Full Lifecycle at a Glance

```
/office-hours          → Brainstorm the idea
/design-consultation   → Create visual identity (new projects)
        ↓
/plan-ceo-review       → Challenge scope & strategy
/plan-design-review    → Review UI/UX plan
/plan-eng-review       → Lock architecture
   (or /autoplan)      → All three in one
        ↓
   [Write code]        → /investigate for debugging, /codex for second opinion
                         /connect-chrome for live browser view, /learn for knowledge
        ↓
/qa                    → Test and fix bugs
/design-review         → Visual polish on live site
/design-shotgun        → Explore multiple design variants
/design-html           → Convert approved mockup to production HTML
/benchmark             → Catch perf regressions
        ↓
/review                → Code review the diff
/cso                   → Security audit (optional)
/ship                  → Version, changelog, PR
        ↓
/land-and-deploy       → Merge, deploy, verify
/canary                → Monitor production
        ↓
/document-release      → Update all docs
/retro                 → Reflect on the sprint
```

---

## Design Skills Cheat Sheet

Since "design" appears in many forms:

| Skill | When | Works On | Does It Fix Code? |
|-------|------|----------|-------------------|
| `/design-consultation` | Starting a new project | Nothing yet — creates the system | No (generates design system) |
| `/plan-design-review` | Plan exists, before coding | The plan/doc | No (rewrites the plan) |
| `/design-shotgun` | Exploring visual directions | Generates multiple variants | No (produces options for you to pick) |
| `/design-review` | After coding, on live site | Running app | **Yes** (atomic commits) |
| `/design-html` | After design is approved | Approved mockup | **Yes** (generates production HTML/CSS) |

---

## Review Skills Cheat Sheet

| Skill | Lens | When |
|-------|------|------|
| `/plan-ceo-review` | CEO/founder — scope, ambition, strategy | Before coding |
| `/plan-design-review` | Designer — UX, visual hierarchy, flows | Before coding |
| `/plan-eng-review` | Eng manager — architecture, edge cases, tests | Before coding |
| `/review` | Code reviewer — bugs, security, structure | After coding, before ship |
| `/cso` | Security officer — OWASP, STRIDE, supply chain | After coding, before ship |
| `/codex` | Second AI opinion — adversarial, independent | Anytime during development |
