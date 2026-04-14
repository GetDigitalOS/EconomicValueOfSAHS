---
version: "v0.01.01"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: low
validation_count: 0
staleness_condition: "Re-examine if: provisioning workflow changes spec file structure; framework init commands change (Vite, Next.js major versions); 2+ bootstrap runs require manual correction."
last_validated: 2026-03-16
---

# Bootstrap Project

Read and execute the `_BOOTSTRAP.md` file in this project's root directory. This file was generated during provisioning and contains step-by-step instructions to scaffold the actual codebase.

## Instructions

1. Read `_BOOTSTRAP.md` from the project root. If it doesn't exist, tell the user and stop.
2. Read `CLAUDE.md`, `PROJECT_BRIEF.md`, `IMPLEMENTATION_ROADMAP.md`, and `TECHNICAL_REFERENCE.md` from the project root or `docs/specs/` to understand the full project context.
3. Execute every step in `_BOOTSTRAP.md` in order. These are real bash commands — run them.
4. After all bootstrap steps are complete, scaffold the actual codebase:
   - Initialize the project with the correct framework (e.g., `npm create vite@latest . -- --template react-ts`, `npx create-next-app@latest . --ts --tailwind`, etc.)
   - Install all dependencies listed in the spec files
   - Create the folder structure described in CLAUDE.md
   - Set up configuration files (tsconfig, tailwind, eslint, prettier, etc.)
   - Create placeholder pages/components matching the roadmap Phase 1
   - Ensure `npm run dev` works before finishing
5. Delete `_BOOTSTRAP.md` from the project root — it's a one-time file.
6. Commit the scaffolded codebase: `git add -A && git commit -m "chore: scaffold codebase via /bootstrap"`
7. Push to origin: `git push`
8. Print a completion summary showing what was created and suggest running `/scaffold` to generate Claude Code harness files (commands, hooks, agents).

## Important

- Do NOT skip steps or reorder them
- If a step fails, stop and report the error — do not continue blindly
- The spec files are the source of truth for stack, structure, and conventions
- After bootstrap, the project should have a working dev server, not just documentation
