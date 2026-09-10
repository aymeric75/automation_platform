# Decisions

This file records only decisions that materially affect the project.

Decisions are append-only: do not delete old decisions. If a decision changes, mark it as `superseded` and reference the new decision.

## Format

```markdown
## D-XXX — Short title

**Status:** accepted | superseded | rejected  
**Date:** YYYY-MM-DD

**Context:** Why was a decision needed?

**Decision:** What was decided?

**Rationale:** Why was this option chosen?

**Consequences:** Main implications, if any.
```

Use the next sequential decision ID.

---

## D-001 — Generic core vs client-specific code

**Status:** accepted  
**Date:** 2026-09-10

**Context:** The platform must be reusable across different clients without accumulating client-specific logic in the core.

**Decision:** The generic core must not contain hard-coded branches for individual clients. Client-specific behavior should be handled through configuration, rules, mappings, connectors or adapters.

**Rationale:** This keeps the platform maintainable and makes new client integrations increasingly reusable.

**Consequences:** New client requirements should first be implemented as configurable or replaceable components rather than direct modifications to the generic core.
