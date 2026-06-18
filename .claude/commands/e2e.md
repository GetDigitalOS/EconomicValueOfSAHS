---
version: "v1.00.00"
owner: "@getdigital2020"
kind: reference-stub
canonical: "C:/dev/project-hub/canonical/commands/e2e.md"
review_cadence: inherits-canonical
---

# /e2e — End-to-End UI/UX Verification  (hub-owned · reference-by-path)

This is a **thin reference stub**, not the methodology. E2E verification is a single
hub-owned function — it is *called*, never copied into projects (ADR-020). Do not paste
the methodology here: editing it in one place must propagate to every project, which only
works if every project points at the one canonical copy instead of holding its own.

## Do this
1. **Read the canonical methodology:** `C:/dev/project-hub/canonical/commands/e2e.md`.
   If that path is unreadable (e.g. a cloud agent with no hub mount), say so and stop —
   do not improvise a substitute method.
2. **Apply it to THIS project**, scaling coverage to this project's tier (from the registry
   entry in `C:/dev/project-hub/registry/projects.json` / `docs/architecture/PROJECT_CLASSIFICATION.md`).
3. The only per-project assets are this repo's **flow script** (`e2e/run.mjs`, or the
   project's existing e2e file) and its gitignored screenshots + findings report. The hub
   owns the *method*; the project owns the *journeys*.
