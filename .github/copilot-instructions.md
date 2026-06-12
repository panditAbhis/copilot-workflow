# Project Coding Standards

Copilot reads this file automatically every session. These rules apply to all code generated in this project.

---

## Spec & Planning

- Write a spec before writing code for any change touching more than one file — use `@spec-writer`
- Break specs into ordered tasks before implementing — use `@planner`
- Every task has acceptance criteria and a verification step
- No task touches more than 5 files
- Surface assumptions explicitly — never silently resolve ambiguous requirements

## Testing

- Write the failing test BEFORE implementation — Red → Green → Refactor
- Bug fixes: write a failing reproduction test first, then fix
- Test pyramid: ~80% unit (no I/O, milliseconds), ~15% integration (real DB, localhost), ~5% E2E (critical paths only)
- Test state and outcomes — never internal method calls or implementation details
- Real implementations > fakes > stubs > mocks — only mock at true system boundaries
- DAMP over DRY in tests — duplication for readability is acceptable
- No skipped tests, no snapshot abuse, no order-dependent tests
- Every test name reads like a specification

## Code Quality

- PR size: ~100 lines ideal, ~300 acceptable, >1000 must split
- Separate refactoring PRs from feature PRs — always
- Names must be descriptive — no `data`, `result`, `temp`, `val` without context
- No nested ternaries, deep callbacks, or "clever" one-liners
- Abstractions earn their existence at the third use case — not the first
- No dead code, no backwards-compat shims for things nobody uses
- No `useCallback`/`useMemo` without profiler evidence of a re-render problem
- Review comments labeled: Critical (blocks merge) / Important (should fix) / Suggestion (optional)

## Git Workflow

- Trunk-based: short-lived branches (1-3 days), always-deployable main
- Atomic commits: one logical change per commit
- Commit format: `type: short description` — feat/fix/refactor/test/docs/chore
- Commit body explains WHY — the diff shows the what
- No formatting changes mixed with behavior changes
- Check staged diff for secrets before every commit
- After any change: document what was changed AND what was intentionally not touched

## Incremental Implementation

- Build one thin vertical slice at a time — not all-DB then all-API then all-UI
- Each slice: implement → test → verify → commit → next slice
- Never write more than ~100 lines without running tests
- Commit after every passing test — commits are rollback save points

## API & Interface Design

- Contract-first: define TypeScript interfaces before implementing
- One error format everywhere: `{ error: { code, message, details? } }`
- All list endpoints paginated from day one — no exceptions
- Validate at system boundaries only — not between internal functions
- Prefer addition over modification — add optional fields, never change or remove existing ones
- Third-party API responses are untrusted — validate shape before use
- REST naming: plural nouns, no verbs (`/tasks` not `/getTasks`)

## Security

- Threat model first: map trust boundaries, name assets, run STRIDE before hardening
- Validate ALL external input at boundaries using schema validation (Zod)
- Parameterize ALL database queries — no string concatenation ever
- Encode all output — never use `innerHTML` with user data
- Passwords: bcrypt/scrypt/argon2 with ≥12 salt rounds
- Sessions: httpOnly + secure + sameSite cookies
- Rate-limit auth endpoints (max 10 req/15 min); general API (100 req/15 min)
- Security headers via helmet (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- CORS: explicit origin allowlist, never wildcard `*`
- SSRF: allowlist outbound URLs, validate resolved IPs are not private/loopback/link-local
- LLM output is untrusted — never pass to `eval`, SQL, shell, `innerHTML`, or file paths
- Never commit secrets — `.env` stays local, `.env.example` committed
- Commit lockfile; CI uses `npm ci` not `npm install`
- New deps: check bundle size + `npm audit` + maintenance activity before adding

## Observability

- Instrument alongside the feature — not after the first incident
- Structured JSON logs with stable event names — no string interpolation
- Correlation/request ID on every log line — mandatory
- RED metrics (Rate, Errors, Duration) on every endpoint and external dependency
- Percentiles only — never averages (averages hide the worst users)
- Alert on symptoms users feel, not on infrastructure causes
- Never log secrets, tokens, passwords, or unredacted PII

## Frontend

- WCAG 2.1 AA on every interactive element — non-negotiable
- Every data-fetching component handles: loading state, error state, empty state
- Mobile-first, responsive from day one — test at 320px/768px/1024px/1440px
- No inline styles, no arbitrary pixel values — use the design system scale
- No components over 200 lines — split them
- Always `<button>` over `<div onClick>`
- No AI aesthetic: no purple gradients, no rounded-2xl on everything, no generic hero sections

## CI/CD

- All gates required: lint → type-check → unit tests → build → security audit
- No gate can be skipped — fix failures, don't disable checks
- Secrets in GitHub Secrets only — never in code or CI config
- Every deployment has a rollback mechanism
- Dependabot or Renovate enabled for weekly dependency updates

## Shipping

- Pre-launch: tests pass, build clean, `npm audit` clear, no `console.log` in production code
- Security: no secrets in code, input validated, auth checks in place, security headers configured
- Accessibility: keyboard navigation works, screen reader conveys structure, contrast ≥ 4.5:1
- Observability in place before release — alerts configured, runbooks linked
- Staged rollout: staging verified before production deploy

---

## Boundaries

**Always:** validate input at boundaries, parameterize queries, encode output, write tests before code, set security headers, use HTTPS, run `npm audit` before release, instrument before shipping, atomic commits, paginate list endpoints

**Ask first:** new auth flows, storing new PII categories, new external integrations, CORS changes, file upload handlers, rate limit changes, elevated permissions, database schema changes, new dependencies

**Never:** commit secrets or `.env`, log passwords/tokens, trust client-side validation as a security boundary, use `eval()`/`innerHTML` with user data, store auth tokens in localStorage, expose stack traces to users, merge without review, skip failing tests, use unbounded fetches in production paths
