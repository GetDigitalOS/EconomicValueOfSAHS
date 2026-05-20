---
version: "v1.01.01"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: high
validation_count: 15
staleness_condition: "Re-examine if: ADR tooling or format standards change in the ecosystem; 2+ projects find the current format insufficient to capture their decision types; Technology Selection Principles in the universal framework change."
last_validated: 2026-03-12
---

# Architecture Decision Record

Create an ADR documenting: $ARGUMENTS

## Instructions

1. Check `docs/architecture/` for existing ADRs to determine the next number.
2. If this is a technology selection decision, reference the Technology Selection Principles from the Universal Web Development Principles:
   - Evaluate against: team fit, community/ecosystem, maturity, performance ceiling, escape hatches, operational cost, architecture alignment
   - Flag any anti-patterns: rÃ©sumÃ©-driven development, premature optimization, framework maximalism, "we might need it" trap, ignoring the exit
3. Write the ADR using this format:

```markdown
## ADR-[NUMBER]: [Title]

**Status:** Proposed
**Date:** [today]
**Deciders:** [who is involved in this decision]

### Context

[What is the issue? What forces are at play? What constraints exist? Why is this decision needed now?]

### Decision

[What are we doing? Be specific about the choice and its scope.]

### Alternatives Considered

#### [Alternative 1]
- **Pros:** [what's good about it]
- **Cons:** [what's concerning]
- **Why not:** [brief reason for rejection]

#### [Alternative 2]
- **Pros:** [what's good about it]
- **Cons:** [what's concerning]
- **Why not:** [brief reason for rejection]

### Consequences

**What becomes easier:**
- [concrete benefit]

**What becomes harder:**
- [concrete tradeoff]

**Risks:**
- [what could go wrong and how we'd detect/mitigate it]
```

4. Save to `docs/architecture/ADR-[NUMBER]-[slugified-title].md`
5. Update `CLAUDE.md` to reference the new ADR in the Architecture Decisions section.
