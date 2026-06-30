---
version: "v0.02.00"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: low
validation_count: 0
staleness_condition: "Re-examine if: provisioning workflow changes spec file structure; framework init commands change (Vite, Next.js major versions); 2+ bootstrap runs require manual correction; the UWP Document Matrix renames the spec files this reads."
last_validated: 2026-06-21
changelog:
  - v0.02.00 (2026-06-21): Aligned the context-read step with the current UWP doc structure — reads `CLAUDE.md` + `docs/specifications/site-spec.md` + `docs/architecture/system-architecture.md` + `docs/planning/roadmap.md` instead of the obsolete root `PROJECT_BRIEF.md`/`IMPLEMENTATION_ROADMAP.md`/`TECHNICAL_REFERENCE.md` and the legacy `docs/specs/` folder (converged to `docs/specifications/` fleet-wide in UWP v4.03.01). Canonicalized the transient file name to `_bootstrap.md` (lowercase, per UWP §Project Initialization Files). Clarified that `/bootstrap` runs once at creation time (after `hub provision`, before the loop is active) so direct git is appropriate here, and that ongoing work uses `dev` + `hub session start` worktrees. Added a pointer to `canonical/references/NEW_PROJECT_CREATION.md`.
  - v0.01.01 (2026-03-16): Baseline.
---

# Bootstrap Project

Read and execute the `_bootstrap.md` file in this project's root directory. This file was generated during provisioning and contains step-by-step instructions to scaffold the actual codebase.

> `/bootstrap` runs **once at project creation time** — right after `hub provision` (which already
> created `dev` + `main`, local and remote) and before the project is enrolled in the autonomous
> build loop. Direct git in the project is therefore appropriate at this stage. *Ongoing* work
> flows through `dev` and `hub session start` worktrees, never the canonical checkout directly
> (ADR-019). Full creation contract: [`../references/NEW_PROJECT_CREATION.md`](../references/NEW_PROJECT_CREATION.md).

## Instructions

1. Read `_bootstrap.md` from the project root. If it doesn't exist, tell the user and stop.
2. Read the project context to understand stack, structure, and conventions (UWP doc structure):
   - `CLAUDE.md` (project entry point)
   - `docs/specifications/site-spec.md` (what to build)
   - `docs/architecture/system-architecture.md` (how it's built)
   - `docs/planning/roadmap.md` (Phase 1 scope)
   - any other `docs/specifications/` and `docs/architecture/` files the tier has.
3. Execute every step in `_bootstrap.md` in order. These are real bash commands — run them.
4. After all bootstrap steps are complete, scaffold the actual codebase:
   - Initialize with the framework the project's tier mandates (`canonical/standards/frontend-framework.md`): **Tier 1–2 → Astro** (`npm create astro@latest`), **Tier 3–4 → Next.js** (`npx create-next-app@latest . --ts --tailwind`) or React+Vite for SPAs. Honor the project's `stack.frontend` / `ADR-002` — including the growth-path override (a Tier 1–2 project marked Tier 3+-bound starts on Next.js).
   - **Astro projects (T1–2):** add `@astrojs/react` (interactive UI = React islands), pin `astro >= 6.4.6`, add a `"format:check": "prettier --check ."` script, build output `dist/` (matches the Cloudflare Pages deploy workflow).
   - Install all dependencies listed in the spec files
   - Create the folder structure described in CLAUDE.md
   - Set up configuration files (tsconfig, tailwind, eslint, prettier, etc.)
   - Create placeholder pages/components matching the roadmap Phase 1
   - Ensure `npm run dev` works before finishing
5. Delete `_bootstrap.md` from the project root — it's a one-time file.
6. Commit the scaffolded codebase: `git add -A && git commit -m "chore: scaffold codebase via /bootstrap"`
7. Push to origin: `git push` (publishes the scaffold to the current branch; both `dev` and `main` already exist on origin from `hub provision`).
8. Print a completion summary showing what was created and suggest running `/scaffold` to generate Claude Code harness files (commands, hooks, agents).

## Important

- Do NOT skip steps or reorder them
- If a step fails, stop and report the error — do not continue blindly
- The spec files are the source of truth for stack, structure, and conventions
- After bootstrap, the project should have a working dev server, not just documentation
