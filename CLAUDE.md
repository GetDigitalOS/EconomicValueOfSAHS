# Economic Value of a Stay-At-Home Spouse

## Overview
Financial-tools calculator quantifying the economic value of a stay-at-home spouse. Early-stage repo — stack per project registry; key commands TBD.

## Classification
- **Tier:** 1 — Static / Marketing
- **Company:** Financial Tools

## Tech Stack (per project registry)
- **Deploy:** Cloudflare Pages
- **SEO:** required

## Key Commands
- _TBD — no `package.json` yet._

## Docs-First Rule (MANDATORY)

This block is the **standing instruction** for every Claude Code session, every sub-agent, and every /command invocation in this workspace.

### Core Rule (enforced on every single interaction)

**For every plan, build, feature, refactor, bugfix, or code change request:**

1. **Immediately and fully read the entire `/docs` folder** before doing anything else.
   - Treat `/docs` as the single source of truth for architecture, decisions, specifications, planning, examples, and reference material.
   - Respect this exact layout (do not create folders outside this structure without explicit approval):
     ```
     docs/
     ├── architecture/
     ├── examples/
     ├── planning/
     ├── reference/
     ├── specifications/
     └── (any .md files explicitly added at top level)
     ```
   - If a `/docs` folder does not yet exist, note that and proceed — but flag it as needing backfill.

2. **Maintain documentation as you go — lean & zero-bloat rule**
   - Only update or add to `/docs` when the change is **meaningful** for a new developer or future team handoff.
   - Prefer **editing an existing file** over creating a new one.
   - Never add filler, repetition, or "nice-to-have" sections.
   - Keep every file concise, scannable, and accurate.
   - If something becomes outdated → mark it `[DEPRECATED — see new location]` or move it to a `docs/archive/` subfolder. Never delete history.
   - After any code change that affects design, specs, architecture, or workflow, **always include** in your response:
     > "Documentation update required: [exact file(s) + 1-sentence summary of what changed]"

3. **Documentation philosophy (enforced)**
   - Living, minimal, high-signal only.
   - One source of truth per topic.
   - No duplicate information between files.
   - Update immediately when the project evolves — never let docs drift from reality.
   - Goal: A new senior developer could be fully productive in <2 hours just by reading `/docs`.

### How this interacts with hub sync

- This `Docs-First Rule` block is **hub-canonical** — `hub sync` may re-insert it if removed.
- Everything else in this `CLAUDE.md` is **project-owned** — hub will never overwrite it.
- If hub updates this block, accept the update. It will never conflict with project-specific content.

## Canonical References

Do NOT copy these into this project. Read from the hub:
- **Universal Web Development Principles**: `C:/dev/project-hub/canonical/references/Universal_Web_Development_Principles.md`
- **Versioning Standards**: `C:/dev/project-hub/canonical/references/VERSIONING_STANDARDS.md`
