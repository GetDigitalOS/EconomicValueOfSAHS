---
version: "v1.02.01"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: established
validation_count: 34
staleness_condition: "Re-examine if: Universal Web Development Principles changes principles or adds/removes tiers; compliance report outputs consistently misalign with actual production quality; a new cross-cutting concern category is added to the principles framework; Project Hub registry schema changes."
last_validated: 2026-03-25
---

# Compliance Check

Audit this project against its tier's principles from the Universal Web Development Principles framework.

## Instructions

1. Determine the project tier: look for `docs/architecture/PROJECT_CLASSIFICATION.md` â€” if it exists, read it. If not, run `/classify` to classify the project first, then proceed.
2. **Verify Project Hub registration:** Read `C:/dev/project-hub/registry/projects.json` and check if the current project's path exists in the projects array. If not registered, add a **Critical** gap to the report: "Project not registered in Project Hub â€” run `/scaffold` to register." If registered, update `last_audited` to today's date and commit.
3. Read the Universal Web Development Principles document.
4. Examine the actual codebase to evaluate compliance against EVERY principle for this tier and all tiers below it.
5. Also check active cross-cutting concerns (Privacy, AI, Design System, Dependencies).
6. Produce the compliance report below.

## Evaluation Method

For each principle, do real verification â€” don't just check if a file exists:

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
# Compliance Report â€” [Project Name]

**Date:** [today]
**Tier:** [X] â€” [Name]
**Framework Version:** Universal Web Development Principles

## Summary

| Status | Count |
|--------|-------|
| âœ… Met | [X] |
| âš ï¸ Partial | [X] |
| âŒ Not Met | [X] |
| â¬œ Not Applicable | [X] |

**Overall Compliance:** [X]% of applicable principles met or partially met

## Critical Gaps (Fix Before Launch)

[List any âŒ items in Security, Accessibility, or Privacy â€” these are non-negotiable]

## Detailed Results

### Tier 1: Security Fundamentals
- âœ… **HTTPS Everywhere** â€” [evidence: Vercel config enforces HTTPS]
- âš ï¸ **Security Headers** â€” [CSP configured but missing X-Frame-Options; see middleware.ts line 23]
- âŒ **Input Validation** â€” [contact form at /contact has no server-side validation]
- âœ… **Dependency Hygiene** â€” [npm audit clean as of today]

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

### Cross-Cutting: Project Hub Registration
- âœ…/âŒ **Registered in Project Hub** â€” Check `C:/dev/project-hub/registry/projects.json` for this project's path
- âœ…/âŒ **Tier matches classification** â€” Registry tier should match `PROJECT_CLASSIFICATION.md`
- âœ…/âŒ **Git remote configured** â€” Registry has a `git_remote` value
- âœ…/âŒ **Dev port assigned** â€” Registry has a `dev_port` in the company's range
- âœ…/âŒ **Status is current** â€” Registry `status` reflects actual project state

## Recommendations (Prioritized)

### ðŸ”´ Critical (Security/Compliance Risk)
1. [specific action with file/line references]

### ðŸŸ¡ Important (Quality/Reliability Risk)
1. [specific action]

### ðŸŸ¢ Recommended (Best Practice)
1. [specific action]

## Next Review

Recommended next compliance check: [date based on project phase]
```

## After the Report

Present the summary to me and highlight:
1. The single most critical gap to fix immediately
2. The total compliance percentage
3. Whether the project is ready for its current phase (development / staging / launch)
