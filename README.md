# copilot-workflow

> How I turned GitHub Copilot into a disciplined engineering team — not just a fast autocomplete.

One-time setup. Works in every repo you create from this template. No re-prompting, no configuration drift, no forgetting the security pass when you're rushing.

## Resources

| | Link |
|---|---|
| Full series | [dev.to/panditabhis](https://dev.to/panditabhis) |
| Template | [github.com/panditAbhis/copilot-workflow](https://github.com/panditAbhis/copilot-workflow) |
| Companion repo | [github.com/panditAbhis/claude-workflow](https://github.com/panditAbhis/claude-workflow) |

---

## Read the Series (in order)

| Part | Title | What you will learn |
|------|-------|---------------------|
| 1 | [Your Copilot Has No Memory. Here's How I Fixed That in 5 Minutes.](https://dev.to/panditabhis/your-copilot-has-no-memory-heres-how-i-fixed-that-in-5-minutes-34l7) | Setup: copilot-instructions.md + agent personas |
| 2 | [Stop Merging Blind: How I Use @code-reviewer Before Every PR](https://dev.to/panditabhis/stop-merging-blind-how-i-use-code-reviewer-before-every-pr-2eji) | 5-axis code review, severity labels, what to fix vs. skip |
| 3 | [Never Fix a Bug Without Proof: The @test-engineer Prove-It Pattern](https://dev.to/panditabhis/never-fix-a-bug-without-proof-the-test-engineer-prove-it-pattern-1cmh) | @test-engineer, the Prove-It Pattern, TDD cycle |
| 4 | [Think Like an Attacker: How I Use @security-auditor Before Every Production Deploy](https://dev.to/panditabhis/think-like-an-attacker-how-i-use-security-auditor-before-every-production-deploy-5434) | @security-auditor, STRIDE, OWASP Top 10 |
| 5 | [One Command to Rule Them All: The /ship Chatmode](https://dev.to/panditabhis/one-command-to-rule-them-all-the-ship-chatmode-that-reviews-audits-and-cleans-before-every-merge-5590) | /ship chatmode, pre-merge fan-out, automated verdict |
| 6 | [Stop Building the Wrong Thing: @spec-writer and @planner](https://dev.to/panditabhis/stop-building-the-wrong-thing-how-i-use-spec-writer-and-planner-before-writing-a-single-line-of-1jd0) | @spec-writer, @planner, /spec chatmode |
| 7 | [A Day in the Life: Complete Session Walkthrough](https://dev.to/panditabhis/a-day-in-the-life-complete-copilot-workflow-session-from-idea-to-merged-pr-4cdb) | Complete 8-step session walkthrough with a real feature |

---

## What is in this repo

```
.github/
  copilot-instructions.md     Coding standards — auto-loaded by Copilot every session
  agents/
    README.md                 When to use which agent and composition rules
    code-reviewer.md          Staff engineer — 5-axis review before every merge
    test-engineer.md          QA engineer — TDD, Prove-It pattern, coverage gaps
    security-auditor.md       Security engineer — OWASP, STRIDE, threat modeling
    web-performance-auditor.md Perf engineer — Core Web Vitals, LCP, INP, CLS
    api-designer.md           API architect — contract-first design, REST patterns
    frontend-engineer.md      Frontend engineer — a11y, WCAG 2.1 AA, no AI aesthetic
    observability-engineer.md SRE — structured logging, RED metrics, alerting
    debugger.md               Staff engineer — root cause, not symptoms
    planner.md                Tech lead — ordered task breakdown, no code
    spec-writer.md            Engineering lead — spec before code, always
    simplifier.md             Staff engineer — clarity without behavior change
    idea-refiner.md           Thinking partner — diverge, converge, one-pager
    adr-writer.md             Engineering lead — decision records, the why
    migration-engineer.md     Staff engineer — deprecate safely, Hyrum's Law
    performance-engineer.md   Perf engineer — backend profiling, N+1, DB indexes
    source-reader.md          Researcher — official docs, version-pinned, cited
    doubter.md                Adversarial reviewer — finds issues, never validates
  chatmodes/
    ship.chatmode.md          Pre-merge: review + security + simplify → SHIP verdict
    spec.chatmode.md          New features: idea → spec → tasks → handoff
    debug.chatmode.md         Bugs: reproduce → root cause → regression test → fix
  workflows/
    ci.yml                    Quality gates: lint, typecheck, tests, build, audit
.vscode/
  mcp.json                    MCP servers: context7 (live docs) + github (PR context)
CONTEXT.md                    Project memory — maintain this to keep Copilot on track
README.md                     You are here
```

---

## The 17 Agents

### Discovery & Planning

| Agent | When to invoke |
|-------|----------------|
| `@idea-refiner` | Vague idea → sharp concept with Not-Doing list. Before @spec-writer. |
| `@spec-writer` | Concept → structured spec with success criteria. Before @planner. |
| `@planner` | Approved spec → ordered task list with acceptance criteria. No code. |
| `@adr-writer` | After any significant technical decision — records the why permanently. |

### Implementation

| Agent | When to invoke |
|-------|----------------|
| `@test-engineer` | Write tests first (TDD). Prove-It Pattern for bugs. Coverage analysis. |
| `@api-designer` | Designing endpoints or module contracts. Contract before implementation. |
| `@frontend-engineer` | Components, layouts, state. WCAG 2.1 AA enforced. Zero AI aesthetic. |
| `@source-reader` | Any framework-specific pattern — fetches real docs, pins to version, cites. |

### Review & Quality

| Agent | When to invoke |
|-------|----------------|
| `@code-reviewer` | Before every PR. 5-axis: correctness, readability, arch, security, perf. |
| `@security-auditor` | Auth, input handling, uploads, external APIs. OWASP Top 10 + STRIDE. |
| `@web-performance-auditor` | Core Web Vitals audit. LCP, INP, CLS. Bundle analysis. |
| `@simplifier` | Post-feature readability pass. Behavior preserved, complexity removed. |
| `@doubter` | Adversarial review of non-trivial decisions. Finds issues — never validates. |

### Operations & Maintenance

| Agent | When to invoke |
|-------|----------------|
| `@observability-engineer` | Logging, metrics, tracing, alerting — instrument before shipping. |
| `@performance-engineer` | Backend profiling, N+1 queries, DB indexes. Measure before optimizing. |
| `@debugger` | Test failures, build errors, unexpected behavior. Root cause first. |
| `@migration-engineer` | Deprecating old APIs or systems. Hyrum's Law. Safe strangler migration. |

---

## The 3 Chatmodes

Chatmodes are automated workflows — one command, multiple agents orchestrated in sequence.

| Chatmode | Trigger | What it does |
|----------|---------|--------------|
| `ship` | Ready to merge | code review + security audit + simplification → single SHIP/NO verdict |
| `spec` | Starting something new | idea refine → spec → task breakdown, gated at each stage |
| `debug` | Something broke | reproduce → localize → root cause → regression test → fix |

Switch chatmodes from the chatmode selector at the top of VS Code Copilot Chat.

---

## The 8-Step Session Workflow

| Step | What | Why |
|------|------|-----|
| 1 | Open `/spec` chatmode for new features | Spec before code — always |
| 2 | `@spec-writer` writes spec, wait for approval | Surface wrong assumptions before building |
| 3 | `@planner` breaks spec into tasks | Know the order and scope before writing code |
| 4 | `@test-engineer` writes failing tests first | Prove what you're building before building it |
| 5 | Implement to make tests pass | Code with a clear target |
| 6 | `@simplifier` for readability | Clean before review, not after |
| 7 | `/ship` chatmode — get SHIP verdict | Code review + security + simplification in one pass |
| 8 | CI green → merge | Automated quality gate confirms everything |

---

## Quick Reference

```
@idea-refiner    vague idea → sharp concept
@spec-writer     concept → structured spec
@planner         spec → ordered task list
@test-engineer   TDD, Prove-It pattern, coverage gaps
@code-reviewer   5-axis review before every PR
@security-auditor OWASP + STRIDE, exploitable vulns only
@simplifier      readability pass, behavior unchanged
@doubter         adversarial review of risky decisions
@debugger        root cause, not symptom

/ship chatmode   pre-merge: review + security + simplify → verdict
/spec chatmode   idea → spec → tasks, gated
/debug chatmode  reproduce → root cause → regression test → fix
```

---

## CONTEXT.md — Copilot's Memory

Claude has a persistent memory system. Copilot doesn't — it starts fresh in every session.

The fix: maintain `CONTEXT.md` at your project root. Copilot reads it via `copilot-instructions.md`'s reference. Update it as the project evolves.

```markdown
# Project Context

## Current sprint
[What you're building right now]

## Recent decisions
- [Decision + reason, date]
- [Decision + reason, date]

## Known issues
- [Issue + workaround or status]

## Architecture notes
- [Key patterns, constraints, non-obvious choices]

## Off-limits
- [Code or systems not to touch without asking]
```

Update it when decisions change. Reference it in Copilot prompts: *"Check CONTEXT.md for current sprint goals."*

---

## 5-Minute Setup

### Prerequisites
- GitHub Copilot subscription (Individual, Business, or Enterprise)
- VS Code + [GitHub Copilot extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
- GitHub Copilot Chat extension

### Step 1 — Use this template

Click **Use this template** → **Create a new repository** → name it → create.

### Step 2 — Verify it's working

Open Copilot Chat (`Cmd+Alt+I` on Mac, `Ctrl+Alt+I` on Windows). Ask:

```
What are the coding standards for this project?
```

Copilot should describe testing, review, and security rules. If it does — you're done.

### Step 3 — (Optional) Enable MCP servers

For live documentation lookup and GitHub PR context:

1. Add `GITHUB_PERSONAL_ACCESS_TOKEN` to your environment (read-only scoped: Contents + Pull requests)
2. Reload VS Code — MCP servers start automatically from `.vscode/mcp.json`

### Step 4 — Try an agent

```
@spec-writer I want to add user notifications when tasks are assigned.
```

---

## Troubleshooting

**Copilot doesn't follow the coding standards**
- Confirm file is at `.github/copilot-instructions.md` (exact path)
- Reload VS Code: `Cmd+Shift+P` → `Developer: Reload Window`
- Check: `github.copilot.chat.codeGeneration.useInstructionFiles` = `true`

**Agent `@` mention not working**
- Requires Copilot Chat — not just inline autocomplete
- Confirm `.github/agents/` files have valid YAML frontmatter (`name` and `description`)
- Restart VS Code

**Chatmode not appearing in selector**
- Confirm files are in `.github/chatmodes/` with `.chatmode.md` extension
- Reload VS Code window

**MCP server not connecting**
- Check `GITHUB_PERSONAL_ACCESS_TOKEN` is set in your shell environment
- Verify token has `Contents: read` and `Pull requests: read` scopes
- Check VS Code Output → MCP for error details

---

## Adapting to Your Stack

Append to `copilot-instructions.md`:

```markdown
## TypeScript
- Strict mode on — no `any` without justification comment
- Zod for all external input validation

## Python
- Type hints on all public functions
- pytest only — no unittest

## Go
- Table-driven tests
- Errors returned, not panicked (except truly unrecoverable)
```

---

## Credit

Agent personas sourced from [Addy Osmani's agent-skills](https://github.com/addyosmani/agent-skills).
Coding standards derived from Google SWE practices and OWASP guidelines.
Companion Claude Code workflow: [claude-workflow](https://github.com/panditAbhis/claude-workflow).

---

*17 agents. 3 chatmodes. One template. The discipline is the point.*
