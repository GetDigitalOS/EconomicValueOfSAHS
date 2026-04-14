---
version: "v1.01.01"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: established
validation_count: 33
staleness_condition: "Re-examine if: Universal Web Development Principles v2 changes principles or adds/removes tiers; compliance report outputs consistently misalign with actual production quality; a new cross-cutting concern category is added to the principles framework."
last_validated: 2026-03-12
---

# Compliance Check

Audit this project against its tier's principles from the Universal Web Development Principles v2 framework.

## Instructions

1. Determine the project tier: look for `docs/architecture/PROJECT_CLASSIFICATION.md` — if it exists, read it. If not, run `/classify` to classify the project first, then proceed.
2. Read the Universal Web Development Principles v2 document.
3. Examine the actual codebase to evaluate compliance against EVERY principle for this tier and all tiers below it.
4. Also check active cross-cutting concerns (Privacy, AI, Design System, Dependencies).
5. Produce the compliance report below.

## Evaluation Method

For each principle, do real verification — don't just check if a file exists:

- **Security Headers**: Check actual deployment config or middleware, not just "there's a next.config.js"
- **Input Validation**: Grep for form handlers and verify validation exists on both client and server
- **Accessibility**: Run axe-core if available, check for semantic HTML, ARIA attributes, heading hierarchy
- **Performance**: Check for image optimization config, lazy loading, code splitting setup
- **Testing**: Check test files exist AND have meaningful coverage, not just empty test files
- **Privacy**: Check for actual privacy policy page, cookie consent implementation, data handling

Use these tools to investigate: Read files, Grep for patterns, Glob for file existence, Bash for running audit commands.

## Report Format

Generate a compliance report as `docs/architecture/COMPLIANCE_REPORT.md`:

```markdown
# Compliance Report — [Project Name]

**Date:** [today]
**Tier:** [X] — [Name]
**Framework Version:** Universal Web Development Principles v2

## Summary

| Status | Count |
|--------|-------|
| ✅ Met | [X] |
| ⚠️ Partial | [X] |
| ❌ Not Met | [X] |
| ⬜ Not Applicable | [X] |

**Overall Compliance:** [X]% of applicable principles met or partially met

## Critical Gaps (Fix Before Launch)

[List any ❌ items in Security, Accessibility, or Privacy — these are non-negotiable]

## Detailed Results

### Tier 1: Security Fundamentals
- ✅ **HTTPS Everywhere** — [evidence: Vercel config enforces HTTPS]
- ⚠️ **Security Headers** — [CSP configured but missing X-Frame-Options; see middleware.ts line 23]
- ❌ **Input Validation** — [contact form at /contact has no server-side validation]
- ✅ **Dependency Hygiene** — [npm audit clean as of today]

### Tier 1: Design System
[continue for each section...]

### Tier 1: Performance
[...]

### Tier 1: Accessibility
[...]

### Tier 1: SEO
[...]

### Tier 1: Code Quality
[...]

### Tier 1: DevOps
[...]

[Continue for Tier 2, 3, 4 sections as applicable]

### Cross-Cutting: Privacy & Data Protection
[Check against the Privacy Checklist by Tier section]

### Cross-Cutting: AI/LLM Integration (if applicable)
[Check against AI Integration by Tier section]

### Cross-Cutting: Dependency Management
[Run npm audit, check lock files committed, evaluate dependency count]

## Recommendations (Prioritized)

### 🔴 Critical (Security/Compliance Risk)
1. [specific action with file/line references]

### 🟡 Important (Quality/Reliability Risk)
1. [specific action]

### 🟢 Recommended (Best Practice)
1. [specific action]

## Next Review

Recommended next compliance check: [date based on project phase]
```

## After the Report

Present the summary to me and highlight:
1. The single most critical gap to fix immediately
2. The total compliance percentage
3. Whether the project is ready for its current phase (development / staging / launch)
