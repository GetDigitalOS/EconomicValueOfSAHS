---
version: "v0.03.00"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: low
validation_count: 0
staleness_condition: "Re-examine if: provisioning workflow changes spec file structure; framework init commands change (Vite, Next.js major versions); 2+ bootstrap runs require manual correction; the UWP Document Matrix renames the spec files this reads."
last_validated: 2026-06-21
changelog:
  - v0.03.00 (2026-06-30): Layer A of the onboarding-enrollment-gate fix (OneManBand follow-up). Step 4 now bakes loop-buildable Next.js gate defaults into the scaffold (`lint: eslint .` not `next lint`; `typecheck: tsc --noEmit`; flat-array `eslint.config.mjs` importing `eslint-config-next/{core-web-vitals,typescript}` directly — no FlatCompat; prettier `endOfLine: auto` for CRLF worktree checkout; pnpm overrides for audit advisories) and a "confirm the gate is green from a clean checkout" step. Step 7 makes the **canonical topology** explicit: the scaffold must land on `default_branch` (ff `main` to `dev`) before enrollment, because the loop bases off `default_branch` — a scaffold stranded on `dev` gates an empty tree (the OneManBand branch-split). Step 8 adds the `hub verify-gate` pre-enroll check. Source: docs/tasks/onboarding-enrollment-gate-preflight-followup.md, canonical/standards/gate-hygiene.md.
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
   - **Next.js projects (T3–4) — ship a loop-buildable gate by default** (these exact defaults are what OneManBand, 2026-06-30, had to be repaired into by hand — bake them in so a new scaffold passes `hub verify-gate` out of the box):
     - `package.json` scripts: `"lint": "eslint ."` (**NOT** `next lint` — Next 16 removed the `next lint` subcommand; it now reads `lint` as a directory and errors), and `"typecheck": "tsc --noEmit"`.
     - `eslint.config.mjs` as a **flat array** that imports the configs directly:
       `import nextCoreWebVitals from "eslint-config-next/core-web-vitals"; import nextTs from "eslint-config-next/typescript";` then `export default [...nextCoreWebVitals, ...nextTs, /* project rules */]`. Do **not** use `FlatCompat().extends("next/...")` — it throws `Converting circular structure to JSON` on ESLint 9 / Next 16.
     - `.prettierrc` with `"endOfLine": "auto"` — the loop's gate worktree is checked out with `core.autocrlf=true` (CRLF), so a default `endOfLine: "lf"` fails `prettier --check` on a tree that passes locally on LF. Also ensure the hub-owned/L0 set is in `.prettierignore` and `eslint.config` ignores (`gate-hygiene.md` §2a).
     - For pnpm projects, add `pnpm.overrides` to `package.json` for known transitive audit advisories (e.g. `"esbuild": ">=0.25.0"`) so a moderate-level audit gate stays green without `--force` churn.
   - Install all dependencies listed in the spec files
   - Create the folder structure described in CLAUDE.md
   - Set up configuration files (tsconfig, tailwind, eslint, prettier, etc.)
   - Create placeholder pages/components matching the roadmap Phase 1
   - Ensure `npm run dev` works before finishing
   - **Confirm the done-gate is green from a clean checkout**, not just locally: the loop runs it on a fresh worktree of the base branch, so run your intended `ci_command` (e.g. `eslint . && tsc --noEmit && <test>`) once before committing. Enrollment will refuse a dirty gate.
5. Delete `_bootstrap.md` from the project root — it's a one-time file.
6. Commit the scaffolded codebase: `git add -A && git commit -m "chore: scaffold codebase via /bootstrap"`
7. **Land the scaffold on `default_branch` (canonical topology — do not skip).** The build loop bases `auto/build/<slug>` off **`default_branch`** (usually `main`) on its first tick and reads both the spec **and** the scaffold from there (`reference_buildloop_enroll_spec_on_default_branch`). If the scaffold is committed only on `dev` while `main` stays empty, the loop gates an **empty tree** and spins `gate-dirty` forever — this is the OneManBand branch-split (2026-06-30). So make `default_branch` carry the scaffold before enrollment:
   - Commit on `dev`, then fast-forward `main` to it: `git checkout main && git merge --ff-only dev && git checkout dev` (both branches then point at one buildable commit). `hub provision` created `dev` and `main` from the same root, so the ff is clean.
   - Push both: `git push origin main dev`.
   - The spec (`autonomous-build.md`) is generated at enrollment onto the loop's ref — but the **scaffold** must already be reachable from `default_branch` here.
8. Print a completion summary showing what was created and suggest running `/scaffold` to generate Claude Code harness files (commands, hooks, agents). Before enrolling in the loop, run `hub verify-gate <name>` and confirm **GATE CLEAN** — enrollment now refuses a dirty gate (it runs the real `ci_command` on a clean `default_branch` worktree).

## Important

- Do NOT skip steps or reorder them
- If a step fails, stop and report the error — do not continue blindly
- The spec files are the source of truth for stack, structure, and conventions
- After bootstrap, the project should have a working dev server, not just documentation
