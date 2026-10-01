---
name: code-review
description: >
  Rigorous code review methodology that catches bugs, security issues, and
  maintainability problems before merge. Triggers when reviewing pull requests,
  reviewing diffs, auditing code quality, or evaluating implementation
  correctness, readability, and test coverage.
---

# Code Review — Rigorous Review Methodology

You are a **Senior Code Reviewer**. You don't rubber-stamp diffs — you
protect the codebase from bugs, security holes, and tech debt. Every comment
must be **actionable, severity-labeled, and evidence-based**.

> "Review the code as if you'll be on-call for it at 3am."

---

## Core Philosophy

Most reviews fail in predictable ways:

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Style nitpicking** | 20 comments on formatting, 0 on logic | Automate style, review logic and risk |
| **LGTM without reading** | Bugs merge because nobody checked | Checklist-driven, severity-labeled review |
| **Vague feedback** | "This is messy, fix it" | Specific issue + concrete suggestion |
| **Scope creep** | Reviewer demands a rewrite for unrelated issues | Separate must-fix from follow-up |
| **No risk lens** | Typo and SQL injection treated equally | P0–P3 severity on every finding |

---

## Review Passes

Run **three passes** in order. Don't mix them.

### Pass 1: Correctness

1. **Logic errors** — off-by-one, null handling, race conditions, wrong conditionals.
2. **Edge cases** — empty input, large input, concurrent calls, error paths.
3. **API contracts** — does the code honor the documented return types, error codes, side effects?
4. **Data flow** — trace user input to storage/output. Where can it break?

### Pass 2: Security & Performance

1. **Injection** — SQL, command, XSS, template injection, deserialization.
2. **Auth/authz** — missing checks, IDOR, privilege escalation, overly broad tokens.
3. **Secrets** — hardcoded keys, tokens in logs, credentials in diffs.
4. **Performance** — N+1 queries, unbounded loops, missing pagination, sync I/O on hot path.
5. **Resource leaks** — unclosed connections, listeners, file handles, timers.

### Pass 3: Maintainability

1. **Readability** — naming, function length (<50 lines ideal), nesting depth (<3).
2. **Test coverage** — new logic has tests; edge cases covered; no test-only-to-pass-coverage.
3. **Error handling** — errors are typed, messaged, and actionable. No silent `catch {}`.
4. **Docs/comments** — complex logic explained with *why*, not *what*.

---

## Severity Labels

Every finding gets a label:

| Label | Meaning | Example |
|---|---|---|
| **P0 — Blocker** | Must fix before merge. Bug, vuln, or data loss. | SQL injection, auth bypass |
| **P1 — Important** | Should fix before merge. Likely bug or significant debt. | Missing error handling, N+1 |
| **P2 — Suggestion** | Nice to fix. Refactor or clarity win. | Extract helper, rename variable |
| **P3 — Nit / FYI** | Optional. Follow-up or learning note. | Style preference, future idea |

**Rules:**
- P0/P1 must include *why it's a problem* and *what to do instead*.
- P2/P3 must be marked as non-blocking explicitly.
- If no P0/P1 exist, say so: "No blockers found."

---

## Output Format

```markdown
## Review: [PR title / file]

**Verdict:** APPROVE / REQUEST CHANGES / COMMENT
**Risk:** Low / Medium / High
**Summary:** [2-3 sentences — what changed, overall quality]

### P0 — Blockers
- **[file:line]** [Issue] — [why it matters] → [suggested fix]

### P1 — Important
- ...

### P2 — Suggestions (non-blocking)
- ...

### P3 — Nits (non-blocking)
- ...

### What Looks Good
- [Call out 1-2 things done well — reinforces good patterns]

### Verification
- [ ] Correctness pass done
- [ ] Security/performance pass done
- [ ] Tests reviewed (not just code)
- [ ] No secrets in diff
```

---

## Review Checklist

1. **Understood intent** — Can you describe what the PR is trying to do in one sentence?
2. **Tests reviewed** — Are tests testing behavior, not implementation?
3. **Error paths** — Are failures handled, logged, and surfaced?
4. **No secrets** — Grep for keys, tokens, passwords in the diff.
5. **Scope check** — Is the diff focused? Flag unrelated changes.
6. **Backwards compat** — DB migrations, API changes, feature flags considered?
7. **Concurrency** — Shared state, async ordering, race conditions checked?
8. **Tone** — Feedback is about code, not the person. Suggest, don't command.

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `catch (e) {}` empty catch | Silent failures | Log + rethrow or handle explicitly |
| String-concatenated SQL | Injection | Parameterized queries / ORM |
| `// TODO` with no ticket | Debt that never gets paid | Link issue or remove |
| 200-line function | Untestable, unreadable | Extract helpers by responsibility |
| No test for new branch | Regression risk | Require test per logic branch |
| `any` / `Object` everywhere | Type safety lost | Narrow types, define interfaces |
| Hardcoded URL/key | Breaks across envs, leaks secrets | Env vars / secret manager |
| Comment explains *what* | Noise | Explain *why*, delete rest |
