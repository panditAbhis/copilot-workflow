# Project Coding Standards

## Testing

- Write failing test BEFORE implementation (Red → Green → Refactor)
- Bug fixes: reproduce bug with a failing test first, then fix
- Test pyramid: ~80% unit, ~15% integration, ~5% E2E
- Unit = no I/O, no network; Integration = real DB, localhost only; E2E = critical user paths only
- Test state/outcomes, not internal method calls
- Each test verifies one concept; name it as a specification
- Real implementations > fakes > stubs > mocks — only mock at uncontrollable external boundaries
- DAMP over DRY in tests; duplication for readability is acceptable
- No skipped tests, no snapshot abuse, no order-dependent tests

## Code Quality

- Review covers 5 axes: correctness, readability, architecture, security, performance
- PR size: ~100 lines ideal, ~300 acceptable, >1000 must split
- Separate refactoring PRs from feature PRs
- Names must be descriptive — no `temp`, `data`, `result` without context
- No nested ternaries, deep callbacks, or "clever" tricks
- Abstractions must earn their complexity — wait for the third use case
- No dead code; no backwards-compat shims unless actively needed
- Dependencies: check bundle size, maintenance status, license, and `npm audit` before adding
- Every PR needs: passing tests, build success, and documented verification story
- Review comments must be labeled: Critical / (unlabeled=required) / Nit / Consider / FYI

## Security

- Threat model first: map trust boundaries, name assets, run STRIDE before hardening
- Validate ALL external input at the boundary using schema validation (e.g., Zod)
- Parameterize ALL database queries — no string concatenation
- Encode all output to prevent XSS; never use `innerHTML` with user data
- Passwords: bcrypt/scrypt/argon2, ≥12 salt rounds
- Sessions: httpOnly + secure + sameSite cookies only
- Rate-limit auth endpoints (max 10 req/15 min); general API (100 req/15 min)
- Security headers: use helmet (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- CORS: explicit origin allowlist, never wildcard `*`
- SSRF: allowlist outbound URLs, validate resolved IPs are not private/loopback/link-local
- LLM output is untrusted input — never pass to `eval`, SQL, shell, `innerHTML`, or file paths
- Commit lockfile; CI installs with `npm ci`

## Boundaries

- **Always:** validate input at boundaries, parameterize queries, encode output, use HTTPS, run `npm audit` before release, write tests before code, set security headers
- **Ask first:** new auth flows, storing new PII categories, new external service integrations, CORS changes, file upload handlers, rate limit changes, elevated permissions
- **Never:** commit secrets or `.env`, log passwords/tokens, trust client-side validation as a security boundary, use `eval()`/`innerHTML` with user data, store auth tokens in localStorage, expose stack traces to users, merge without review, skip tests to make suite pass
