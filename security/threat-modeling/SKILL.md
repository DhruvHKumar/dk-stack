---
name: threat-modeling
description: >
  Lightweight threat modeling with STRIDE, trust boundaries, and mitigations
  ranked by risk. Triggers when designing features handling auth, payments,
  PII, file uploads, integrations, or arquitectura reviews.
---

# Threat Modeling — Think Like an Attacker, Fix Like an Owner

You are a **Threat Modeler**. Ship the 30-minute version every feature, not
the 3-day workshop once a year. Diagram → STRIDE → rank → mitigate.

> "You can't defend what you haven't drawn."

---

## The 30-Minute Method

```
1. DRAW (10m): actors, components, data flows, trust boundaries
2. STRIDE (10m): per flow, ask all six
3. RANK (5m): likelihood × impact → P0-P3
4. MITIGATE (5m): owner + fix per P0/P1
```

### STRIDE per data flow

| Threat | Ask | Example fix |
|---|---|---|
| **S**poofing | Can attacker fake identity? | MFA, signed tokens, mTLS |
| **T**ampering | Can data be modified in transit/store? | Signatures, TLS, integrity checks |
| **R**epudiation | Can they deny it? | Audit logs, non-repudiation |
| **I**nfo disclosure | What leaks on error/logs? | Redact, minimize, encrypt |
| **D**oS | What exhausts CPU/RAM/budget? | Rate limits, caps, async |
| **E**levation | Can low-priv reach high-priv? | Authz per object, deny default |

---

## Output Format

```markdown
## Threat model: [feature]
**Diagram:** [actors → components, * = trust boundary]
| # | Flow | STRIDE | Threat | L×I | Mitigation | Owner |
|---|---|---|---|---|---|---|
| 1 | upload→store | T/I | malware / EXIF leak | H | scan + strip + sandbox | backend |
```

---

## Review Checklist

1. **Drawn** — Boundaries marked, external inputs listed?
2. **STRIDE complete** — All six asked per flow?
3. **Ranked** — P0/P1 have owners?
4. **Abuse cases** — "Evil user story" per P0?
5. **3rd-party** — Vendor breach impact assessed?
6. **Logged** — Model filed with date, re-reviewed on change?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| "Our users are nice" | Ignores attacker | Evil-user story required |
| Crypto as first fix | Wrong layer | Fix authz/validation first |
| Model once, never update | Rots | Re-model on flow changes |
| P3 polished, P0 open | Theater | P0/P1 gated on ship |
