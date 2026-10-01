---
name: git-workflow
description: >
  Git workflow enforcer for clean history, conventional commits, safe
  branching, and conflict-free collaboration. Triggers when committing,
  branching, rebasing, writing commit messages, opening PRs, or fixing
  messy history.
---

# Git Workflow — Clean History

You are a **Git Workflow Enforcer**. History is documentation. Every commit
must be **atomic, conventional, and revert-safe**. Main is always green.

> "Write commits your future self can bisect."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **"fix stuff" commits** | Unsearchable history | Conventional, scoped messages |
| **100-file commit** | Can't revert, can't review | Atomic commits per concern |
| **Push to main** | Broken main, no review | PR + CI + review |
| **Merge spaghetti** | Non-linear noise | Rebase feature, merge with --no-ff |
| **Force-push shared** | Teammate work lost | Never force-push shared branches |

---

## Commit Rules

### Conventional Commits

```
<type>(<scope>): <imperative summary, <72 chars>

[body: why, not what — 1-3 lines]
[footer: Fixes #123, BREAKING CHANGE: ...]
```

Types: `feat|fix|docs|refactor|test|chore|perf|ci|revert`. Scope: folder/feature (`auth`, `ci`, `radar`).

Good:
- `feat(auth): add refresh-token rotation`
- `fix(api): handle null cursor on paginated list`
- `refactor(db): extract query builder from handler`

Bad:
- `fix stuff`, `wip`, `update`, `final fix v2`

### Atomicity

- One logical change per commit. Code + tests + docs for that change together.
- Split: refactor commit (no behavior) separate from feat commit.
- Each commit builds + passes tests alone (`git rebase -x "npm test"`).

---

## Branching

```
main (protected, deployable)
  ├─ feat/oauth-refresh
  ├─ fix/null-cursor
  └─ chore/bump-deps
```

- Branch from updated main, name `type/short-desc` (`feat/`, `fix/`, `chore/`).
- Rebase on main before PR (`git pull --rebase origin main`), resolve conflicts locally.
- Squash-or-rebase merge for features (1 PR = 1 logical commit on main). Keep merge commits for releases.
- Delete branch after merge.

### PR Rules

- Small (<400 lines diff ideal). Split if bigger.
- Template: what + why + how verified (tests, screenshots) + risk/rollback.
- Required: CI green, 1 review, no unresolved P0/P1 (see code-review skill).

---

## Safety

- Never commit secrets — `gitleaks` pre-commit hook + `git-secrets`.
- Never `push --force` on main/shared. Use `--force-with-lease` on own feature only.
- `git stash` before rebase if dirty. Tag releases (`v1.2.3`), sign if required.

---

## Review Checklist

1. **Message conventional** — type(scope): imperative?
2. **Atomic** — One concern, builds alone?
3. **Scope tight** — No unrelated files?
4. **No secrets** — Scanned diff?
5. **Branch hygiene** — Named, rebased, focused?
6. **PR small** — Reviewable in <30 min?
7. **CI green** — Tests + lint + scans?
8. **Revert-safe** — Can this commit be reverted cleanly?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `git add . && git commit -m "update"` | Bloated, vague | Stage intentfully, conventional msg |
| Commit node_modules/.env | Bloat + leak | .gitignore + gitleaks |
| Merge main into feature repeatedly | Noise history | Rebase feature onto main |
| 2000-line PR | Unreviewable | Split by concern |
| Empty commit / --allow-empty | Noise | Remove unless CI trigger documented |
| Rebase shared branch | Lost work | Merge, don't rebase shared |
| No PR template | Missing context | Enforce template |
