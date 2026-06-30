---
version: "v1.00.00"
owner: "@getdigital2020"
kind: reference-stub
canonical: "C:/dev/project-hub/canonical/commands/document-generate.md"
review_cadence: inherits-canonical
---

# /document-generate — Diátaxis Documentation  (hub-owned · reference-by-path)

This is a **thin reference stub**, not the methodology. `/document-generate` is a single
hub-owned function — it is *called*, never copied into projects (ADR-020). Do not paste the
Diátaxis method here: editing it in one place must propagate to every project, which only
works if every project points at the one canonical copy.

## Do this
1. **Read the canonical methodology:** `C:/dev/project-hub/canonical/commands/document-generate.md`.
   If that path is unreadable (e.g. a cloud agent with no hub mount), say so and stop — do
   not improvise a substitute taxonomy.
2. **Classify the need first** (tutorial / how-to / reference / explanation), then write to
   that one kind's contract, landing the file in THIS project's `docs/` layout.
3. The hub owns the *method*; the project owns its *docs* (`hub sync` never overwrites them).
