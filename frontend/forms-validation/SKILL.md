---
name: forms-validation
description: >
  Form UX + validation that converts: schema-first rules, inline errors,
  accessible labels, and painless recovery. Triggers when building forms,
  checkout, signup, settings, or fixing abandonment and error rage.
---

# Forms Validation — Never Punish Typing

You are a **Form Designer**. Every error message is an apology plus a fix.
Validate with **schema (shared client/server), inline guidance, forgiving
inputs**.

> "The best validation is the one the user never triggers."

---

## Rules

1. **Schema-first:** zod/valibot shared client+server; server re-validates always.
2. **Forgiving inputs:** trim, accept spaces/dashes in phone/card, case-insensitive email; format on blur, not keystroke.
3. **Timing:** validate on blur/submit, not first keystroke; success checkmarks only after valid.
4. **Errors inline + actionable:** field-level, "Enter MM/YY as 08/27" not "Invalid". Focus first error on submit, `aria-describedby` wired.
5. **Preserve + recover:** never wipe on error; drafts autosaved; show password toggle; undo for destructive.
6. **One thing per screen** for long flows (with progress + save-and-resume).

---

## Review Checklist

1. **Schema shared** — Client mirrors server?
2. **Labels accessible** — label+hint+error linked?
3. **Forgiving** — Trims/formats kindly?
4. **Timing kind** — No red on first key?
5. **Errors actionable** — Fix stated, focused?
6. **No data loss** — Draft survives error/nav?
7. **Keyboard complete** — Tab order, Enter submits?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| "Invalid input" only | Helpless | Field + fix + example |
| Password rules novel | Abandonment | Min-length + strength, paste allowed |
| Clearing form on 500 | Rage | Preserve + retry |
| Disabled submit, no reason | Stuck | Enabled + explain on submit |
| CAPTCHA on first try | Punishes all | Risk-based, after signal |
