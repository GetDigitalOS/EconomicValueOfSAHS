---
version: "v1.00.00"
owner: "@getdigital2020"
kind: reference-stub
canonical: "C:/dev/project-hub/canonical/commands/learn.md"
review_cadence: inherits-canonical
---

# /learn — Per-Project Persistent Memory  (hub-owned · reference-by-path)

This is a **thin reference stub**, not the methodology. `/learn` is a single hub-owned
function — it is *called*, never copied into projects (ADR-020). Do not paste the method
here: editing it in one place must propagate to every project, which only works if every
project points at the one canonical copy.

## Do this
1. **Read the canonical methodology:** `C:/dev/project-hub/canonical/commands/learn.md`.
   If that path is unreadable (e.g. a cloud agent with no hub mount), say so and stop —
   do not improvise a substitute memory format.
2. **Operate on THIS project's `.claude/memory/`** (the `MEMORY.md` index + one-fact-per-file
   topic files). Capture, recall, and curate per the canonical rules.
3. The hub owns the *method*; the project owns its *memory* (`.claude/memory/` is
   project-owned — `hub sync` never overwrites it). The build loop's Reflect step writes
   `type: reflection` memories into the same store when the project opts in.
