# copilot-workflow

> How I turned GitHub Copilot into a disciplined pair programmer — not just an autocomplete engine.

## Resources

| | Link |
|---|---|
| Full series | [dev.to/panditabhis](https://dev.to/panditabhis) |
| Workflow files | [github.com/panditAbhis/copilot-workflow](https://github.com/panditAbhis/copilot-workflow) |

---

## Read the Series (in order)

| Part | Title | What you will learn |
|------|-------|---------------------|
| 1 | Coming soon | Setting up agent personas and coding standards in Copilot |
| 2 | Coming soon | Using @code-reviewer, @test-engineer, @security-auditor in Copilot Chat |
| 3 | Coming soon | Building agentic workflows on top of this template |

---

## What is in this repo

```
.github/
  copilot-instructions.md   project coding standards (auto-loaded by Copilot)
  agents/
    code-reviewer.md        staff engineer persona — 5-axis code review
    test-engineer.md        QA engineer persona — TDD + coverage analysis
    security-auditor.md     security engineer persona — OWASP + threat modeling
```

## Use this template

1. Click **Use this template** → **Create a new repository**
2. The `.github/` folder is auto-detected by GitHub Copilot
3. `copilot-instructions.md` loads automatically on every Copilot session
4. Agent personas activate in Copilot Chat via `@` mention

## Invoking agents in Copilot Chat

| Agent | Invoke | Purpose |
|-------|--------|---------|
| Code Reviewer | `@code-reviewer Review this PR` | 5-axis review: correctness, readability, arch, security, perf |
| Test Engineer | `@test-engineer Write tests for this function` | TDD, coverage gaps, Prove-It pattern for bugs |
| Security Auditor | `@security-auditor Audit this auth middleware` | OWASP Top 10, threat modeling, STRIDE |

---

*Template. Use it, fork it, adapt it. The discipline is the point.*
