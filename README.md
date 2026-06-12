# copilot-workflow

> How I turned GitHub Copilot into a disciplined pair programmer — not just an autocomplete engine.

One-time setup. Works in every repo you create from this template. No re-configuration, no repeated prompting.

## Resources

| | Link |
|---|---|
| Full series | [dev.to/panditabhis](https://dev.to/panditabhis) |
| Template | [github.com/panditAbhis/copilot-workflow](https://github.com/panditAbhis/copilot-workflow) |

---

## Why this exists

Vanilla Copilot is generic. It doesn't know your testing philosophy, your security rules, or your review standards. Every session you either prompt it into shape or accept mediocre output.

This template fixes that permanently:
- **`copilot-instructions.md`** — Copilot reads this automatically. Your rules, every session, zero effort.
- **Agent personas** — Switch Copilot into specialist mode (reviewer, tester, security auditor) with one `@` mention.

---

## 5-minute setup

### Prerequisites
- GitHub Copilot active on your account (Individual, Business, or Enterprise)
- VS Code with the [GitHub Copilot extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) installed
- GitHub Copilot Chat extension installed

### Step 1 — Create your repo from this template

1. Click **Use this template** → **Create a new repository**
2. Name it, set visibility, click **Create repository**

That's it. The `.github/` folder is copied into your new repo and Copilot picks it up immediately.

### Step 2 — Verify Copilot is reading your instructions

Open Copilot Chat in VS Code and ask:

```
What are the coding standards for this project?
```

Copilot should describe the testing pyramid, 5-axis review, and security rules. If it does — you're done.

### Step 3 — Use the agents

In Copilot Chat, prefix your message with the agent name:

```
@code-reviewer Review the changes in auth/middleware.ts
@test-engineer Write tests for the createUser function
@security-auditor Audit this file upload handler for vulnerabilities
```

No installation. No configuration. The `.github/agents/` folder is auto-discovered.

---

## What's in this repo

```
.github/
  copilot-instructions.md     Your project standards — auto-loaded by Copilot every session
  agents/
    README.md                 How agents compose and when to use each one
    code-reviewer.md          Staff engineer persona — 5-axis code review
    test-engineer.md          QA engineer persona — TDD + coverage analysis
    security-auditor.md       Security engineer persona — OWASP + threat modeling
README.md                     You are here
```

---

## The three agents

### `@code-reviewer` — Staff Engineer

Reviews every change across five dimensions: **correctness, readability, architecture, security, performance**.

Every finding is labeled — Critical (blocks merge), Important (should fix), Suggestion (optional). You always know what's required vs. what's nice to have.

**When to use:**
- Before opening a PR
- After a large refactor
- When another AI generated code you need to evaluate

### `@test-engineer` — QA Engineer

Designs test suites, writes tests, and finds coverage gaps. Follows the **Prove-It Pattern** for bugs: write a failing test that reproduces the bug *before* implementing the fix. The test is the proof the bug existed and is now gone.

**When to use:**
- Starting a new feature (write tests first)
- Got a bug report (reproduce it with a test first)
- Unsure if your test suite covers the right things

### `@security-auditor` — Security Engineer

Starts from trust boundaries, runs STRIDE analysis, maps findings to OWASP Top 10. Only flags **exploitable** vulnerabilities — not theoretical risks. Every finding includes a proof-of-concept and a specific fix.

**When to use:**
- Any code that handles user input
- Auth flows, file uploads, webhooks, external API integrations
- Before shipping anything to production

---

## The coding standards (what Copilot knows)

These rules are in `copilot-instructions.md` and apply automatically.

### Testing
- Write the failing test **before** the implementation — always
- Bug fix = reproduction test first, then fix
- Test what the code **does**, not how it does it internally
- Real implementations over mocks — only mock at true system boundaries (network, database)
- 80% unit / 15% integration / 5% E2E

### Code quality
- PR size: ~100 lines ideal, ~300 acceptable, >1000 must be split
- Refactoring PRs are separate from feature PRs — always
- No dead code, no backwards-compat shims for things nobody uses
- Abstractions earn their existence at the **third** use case, not the first

### Security
- Treat every external input as hostile — validate at the boundary
- Parameterize all database queries — no string concatenation, ever
- Never commit secrets — `.env` stays local, `.env.example` gets committed
- LLM output is untrusted input — never pass to `eval`, SQL, shell, or `innerHTML`

---

## Adapting this to your project

### Add language-specific rules
Append to `copilot-instructions.md`:

```markdown
## TypeScript
- Strict mode always on — no `any` without an explicit justification comment
- Zod for all external input validation

## Python
- Type hints on all public functions
- pytest only — no unittest
```

### Add more agents
Create a new `.md` file in `.github/agents/` with this frontmatter:

```markdown
---
name: your-agent-name
description: One sentence — what this agent does and when to invoke it
---

# Agent Name
[Instructions for the agent...]
```

Copilot discovers it automatically. No registration needed.

### Grow into agentic workflows
Future additions (coming as this repo evolves):
- `/review` slash command — fans out to all three agents in parallel
- `/ship` command — runs review + tests + security audit before merge
- GitHub Actions for automated Copilot review on every PR

---

## Troubleshooting

**Copilot doesn't seem to know my coding standards**
- Confirm the file is at `.github/copilot-instructions.md` (exact path)
- Reload VS Code window: `Cmd+Shift+P` → `Developer: Reload Window`
- Check Copilot Chat settings: `github.copilot.chat.codeGeneration.useInstructionFiles` must be `true` (it's the default)

**Agent `@` mention not working**
- Agents require Copilot Chat — not just inline Copilot autocomplete
- Confirm `.github/agents/` files have valid YAML frontmatter (`name` and `description` fields)
- Try restarting VS Code

**`@code-reviewer` gives generic output**
- Give it context: `@code-reviewer Review auth/login.ts — focus on the session management changes`
- Paste the diff directly if it's a small change

---

## Credit

Agent personas sourced from [Addy Osmani's agent-skills](https://github.com/addyosmani/agent-skills).
Coding standards derived from Google's SWE practices and OWASP guidelines.

---

*Template. Use it, fork it, adapt it. The discipline is the point.*
