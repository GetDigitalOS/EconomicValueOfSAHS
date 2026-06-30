---
version: "v1.01.01"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: established
staleness_condition: "Re-examine if: the dashboard's project-completion reader (src/lib/project-completion.ts) changes the manifest schema or path; the ROADMAP_CANDIDATES list diverges; or the phase-status enum changes."
last_validated: 2026-06-20
changelog:
  - v1.01.01 (2026-06-20): Baseline. Guided authoring of docs/planning/progress.json for the dashboard's curated (weighted, blocker-aware) completeness metric.
---

# Track Project Completeness

Author (or update) this project's **curated completeness manifest** —
`docs/planning/progress.json` — through a short interview with me. The Project
Hub dashboard reads this file and shows a weighted, blocker-aware completeness
number instead of the default raw checkbox count.

Walk through these steps interactively. Ask one thing at a time and wait for my
answer before moving on.

## Step 1: Locate and read the roadmap

Find the first of these that exists in this repo and read it:

1. `docs/planning/roadmap.md`
2. `docs/roadmap.md`
3. `roadmap.md`
4. `implementation-roadmap.md` / `docs/planning/implementation-roadmap.md`
5. `docs/planning/NEXT_SESSION.md`
6. `SPRINT_PLAN.md` / `docs/SPRINT_PLAN.md`

(This is the same candidate list the dashboard's auto metric uses.) Also read
`CLAUDE.md` "Current Phase" / status sections if present — they often describe
which phases are done vs. externally blocked. If no roadmap exists, tell me, and
ask whether to proceed by defining phases from scratch.

## Step 2: Propose a phase list

From the roadmap structure (phase headings, sprint sections, or major
deliverables), propose a list of **phases** — typically 5–12. For each, suggest:

- a short **name**,
- a **weight** (relative effort/importance; integers, default 1 — bigger phases
  get more), and
- a **status** guess from the roadmap evidence.

Present them as a table and ask me to correct names, weights, and statuses.
Don't pad the list — coarse and honest beats granular and fabricated.

## Step 3: Confirm status for each phase

For each phase, confirm one status. **Be precise about the distinction that the
whole point of this manifest is to capture** — what is unbuilt vs. what is merely
waiting on something outside the team:

| status | meaning | counts toward headline? |
|---|---|---|
| `done` | shipped / complete | yes (1.0) |
| `in_progress` | actively being built | yes (supply a `fraction` 0–1, default 0.5) |
| `not_started` | buildable, not begun | yes (0.0) |
| `blocked_external` | waiting on a third party (API access, vendor, credentials) | **no — excluded from headline, shown separately** |
| `blocked_human` | waiting on a human action (sign-off, manual config, decision) | **no — excluded from headline, shown separately** |

For `in_progress`, ask for an approximate `fraction` (e.g. 0.5). Do **not** guess
whether work is blocked — ask me. That judgment is exactly what the human-assisted
tier exists for.

## Step 4: Write the manifest

Write `docs/planning/progress.json` with this exact shape (no extra fields):

```json
{
  "version": 1,
  "phases": [
    { "name": "Middleware Foundation", "weight": 3, "status": "done" },
    { "name": "Altruist + AdvicePay",  "weight": 5, "status": "in_progress", "fraction": 0.9 },
    { "name": "Wealthbox config",      "weight": 2, "status": "blocked_external" },
    { "name": "Client sign-off",       "weight": 1, "status": "blocked_human" }
  ]
}
```

Rules:
- `name` (string) and `status` (one of the five enum values) are required.
- `weight` is a positive number; omit or it defaults to 1.
- `fraction` (0–1) only applies to `in_progress`; ignore it elsewhere.
- The dashboard computes
  `headline = Σ(weight·fraction of counted phases) / Σ(weight of counted phases)`,
  where counted = `done | in_progress | not_started`. Blocked phases are surfaced
  as a separate "N blocked" note, not in the headline.

After writing, show me the computed headline percentage and the blocked count so
I can sanity-check it, and remind me the file is committed to this repo (the
dashboard reads it from the working tree). Re-run this command anytime to update.
