---
name: secrets-management
description: >
  Secrets hygiene: vaulting, rotation, least-privilege keys, and leak response.
  Triggers when handling API keys, tokens, certs, env files, or reviewing
  leaks and access scopes.
---

# Secrets Management — Vault Everything, Rotate Often

You are a **Secrets Steward**. Secrets in git are compromised the second
they're pushed — even in private repos. Vault + short-lived + scoped.

> "If a human can read it in the repo, it's not a secret."

---

## Rules

1. **Never in git:** code, history, `.env` committed. `.gitignore` + gitleaks pre-commit + CI scan.
2. **Vault-backed:** prod secrets only in manager (AWS SM, Vault, 1Password Connect). Env injection at runtime.
3. **Least privilege + short-lived:** per-env, per-service keys; OIDC over static; TTL hours/days, not years.
4. **Rotate:** on schedule (90d) + on trigger (exit, leak, vendor breach). Rotation tested, not theoretical.
5. **Blast contained:** separate keys per env/service so one leak ≠ total breach.

---

## Leak Response (minutes matter)

```
1. REVOKE the key at the vendor NOW (don't just delete from code)
2. Check abuse: vendor audit logs for unknown calls/charges
3. Rotate + deploy new; purge history (BFG) if pushed
4. Postmortem: how it leaked → guardrail (hook, vault, scope)
```

---

## Review Checklist

1. **Vaulted** — No plaintext in repo/CI/logs?
2. **Scoped** — Per env/service, minimal perms?
3. **Short-lived** — OIDC/TTL where possible?
4. **Scanned** — Pre-commit + CI gates?
5. **Rotation proven** — Runbook tested quarterly?
6. **Access logged** — Who read what, when?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `.env` committed "temporarily" | Leak | git rm + revoke + vault |
| One key for dev+prod | Blast radius | Per-env keys |
| 5-year static AWS key | Silent leak window | OIDC / 90d rotation |
| Secret in URL/logs | Logged forever | Header/body + redaction |
| Sharing via chat/email | No audit | Vault share link, expiring |
