# Project Context

> Copilot's memory. Update this as the project evolves.
> Reference it in prompts: "Check CONTEXT.md for current goals."

---

## Current Sprint

[What you're building right now — be specific]

Example:
> Adding email notifications for task assignments. Scope: backend only (SMTP via Resend), no frontend changes this sprint.

---

## Recent Decisions

| Date | Decision | Reason |
|------|----------|--------|
| [date] | [decision] | [why] |

Example:
| 2026-06-12 | Use Resend for email, not Nodemailer | Resend has better deliverability and a simpler API for transactional email |
| 2026-06-10 | Zod for all input validation | Consistent with existing auth middleware patterns |

---

## Known Issues

- [Issue description + workaround or status]

Example:
- Task search is slow on tables > 10k rows — full-text index not yet added (tracked, next sprint)
- File uploads fail on Safari 16 — known browser bug, workaround: force multipart/form-data

---

## Architecture Notes

Key patterns and non-obvious choices agents need to know:

- [Pattern or constraint]

Example:
- All background jobs go through BullMQ — never use raw setTimeout for async work
- Database access only through the service layer — never query the DB directly from route handlers
- Feature flags via environment variables — no SDK, keep it simple

---

## Off-Limits

Code or systems not to touch without explicit discussion:

- [Area + reason]

Example:
- `src/billing/` — owned by payments team, any change needs their review
- `migrations/` — never edit existing migrations, always create new ones
- `.env.production` — read-only, never modify, never commit

---

## Open Questions

Things that need a decision before proceeding:

- [ ] [Question]

Example:
- [ ] Should notifications be real-time (WebSocket) or polling? Decision needed before sprint 3.

---

## How to Use This File

1. **Update Sprint** at the start of each sprint
2. **Log Decisions** when you make a significant technical choice — date + reason
3. **Add Known Issues** when you discover a bug or limitation you're not fixing now
4. **Reference in prompts:** `@spec-writer Check CONTEXT.md for current sprint scope before writing the spec.`

Copilot reads `copilot-instructions.md` automatically. Mention `CONTEXT.md` explicitly in prompts where current sprint context matters.
