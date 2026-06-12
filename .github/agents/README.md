# Agents

11 specialist personas for GitHub Copilot Chat. Each is an expert in one domain. Independent specialists — they do not call each other. Orchestration is yours.

---

## Full Agent Roster

| Agent | Persona | When to invoke |
|-------|---------|----------------|
| `@spec-writer` | Engineering Lead | New feature, vague requirements, multi-file change — writes spec first, no code |
| `@planner` | Tech Lead | After spec approved — breaks work into ordered, verifiable tasks |
| `@code-reviewer` | Staff Engineer | Before every PR — 5-axis review: correctness, readability, arch, security, perf |
| `@test-engineer` | QA Engineer | TDD, coverage gaps, Prove-It pattern for bugs |
| `@security-auditor` | Security Engineer | Auth, input handling, file uploads, external integrations — OWASP + STRIDE |
| `@api-designer` | API Architect | Designing endpoints, TypeScript interfaces, module contracts — contract first |
| `@frontend-engineer` | Frontend Engineer | Components, layouts, accessibility, state management — zero AI aesthetic |
| `@observability-engineer` | SRE | Logging, metrics, tracing, alerting before shipping |
| `@debugger` | Staff Engineer | Test failures, build errors, unexpected behavior — root cause, not symptom |
| `@simplifier` | Staff Engineer | Post-feature readability pass — preserves behavior, improves clarity |
| `@web-performance-auditor` | Perf Engineer | Core Web Vitals, LCP, INP, CLS, bundle analysis |

---

## Decision Guide

```
Starting something new?
  → @spec-writer first → @planner after approval → implement

Code written, tests pass, opening a PR?
  → @code-reviewer always
  → + @security-auditor if it touches auth, input, uploads, APIs
  → + @simplifier if it feels complex

Got a bug report?
  → @debugger to reproduce and find root cause
  → @test-engineer to write the Prove-It reproduction test

Building a frontend component?
  → @frontend-engineer for a11y + design system + state

Designing a new API?
  → @api-designer before writing any implementation

Shipping to production?
  → @observability-engineer to verify telemetry is in place

Slow page? Poor Core Web Vitals?
  → @web-performance-auditor
```

---

## Composition Rules

Agents are **independent specialists**. They never invoke each other.

- `@code-reviewer` flags a security issue → you invoke `@security-auditor` for a deeper pass
- `@security-auditor` recommends adding tests → you invoke `@test-engineer`
- `@planner` produces tasks → you implement them with `@test-engineer` guiding TDD

This is intentional. An agent that delegates to another agent is unpredictable and hard to debug. Orchestration belongs to you (or to a future slash command).

---

## Example Invocations

```
@spec-writer Write a spec for adding email notification support to the task system
@planner Break this approved spec into implementation tasks
@code-reviewer Review the changes in src/api/tasks.ts before I open the PR
@test-engineer I have a bug — completed tasks still show in the active list. Write a failing test.
@security-auditor Audit src/routes/upload.ts for OWASP Top 10 and SSRF vulnerabilities
@api-designer Design the REST contract for a comments sub-resource on tasks
@frontend-engineer Review TaskList.tsx — check accessibility and state management
@observability-engineer What telemetry should I add to the new payment retry feature?
@debugger The CI test "creates a task with default status" is failing after my auth changes
@simplifier Do a readability pass on src/services/task.ts — tests all pass
@web-performance-auditor Audit the dashboard page for Core Web Vitals issues
```

---

## Adding a New Agent

Create a `.md` file in this directory with frontmatter:

```markdown
---
name: agent-name
description: One sentence — what it does and when to invoke it
---

# Agent Title

[Role, approach, rules, output format]
```

Copilot discovers it automatically. No registration needed.

---

## The Automated Flow (GitHub Actions)

Every PR triggers `.github/workflows/ci.yml` automatically:

```
PR opened
  → Lint + Type Check (parallel)
  → Unit & Integration Tests (parallel)
  → Build (after lint + tests pass)
  → Security Audit (parallel)
```

All gates are required. Failures block merge. Fix the failure — don't disable the gate.
