---
version: "v1.03.00"
owner: "@getdigital2020"
review_cadence: quarterly
derived_from: ["project-hub"]
confidence: high
validation_count: 33
staleness_condition: "Re-examine if: Claude Code project setup mechanics change significantly; hub sync file ownership rules change; setup fails or requires manual correction for 2+ consecutive projects; the retrofit workflow adds new required steps."
last_validated: 2026-07-01
changelog: "v1.03.00 (2026-07-01, RET-2026-0009): reconcile Docs-First block + docs-maintenance layout with the verify-docs UWP matrix — add `operations/` (Tier 3+) and `adr/` to the approved structure, add a Tier-3+ Operational-capture rule so runbooks/incident-response accrete from real ops work, and permit signal-keyed skeletons for Tier-4 runbooks in backfill."
---

# /setup-docs

Perform this exact sequence of one-time setup steps for this project in Claude Code:

---

## Step 1: Handle CLAUDE.md

**CLAUDE.md is the persistent system prompt for every Claude Code session in this workspace.** It is read automatically at the start of every session.

- If `CLAUDE.md` does not exist → create it starting with `# {PROJECT_NAME}` followed immediately by the mandatory block below.
- If `CLAUDE.md` already exists:
  - Check if it already contains the exact heading `## Docs-First Rule (MANDATORY)`.
  - If yes → do nothing to that section (don't duplicate).
  - If no → insert the block below immediately after the first `# ` or `## ` heading (append at end if no heading exists).

### The block to insert (do not alter any wording):

```
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
     ├── adr/               (Architecture Decision Records — Tier 1+)
     ├── examples/
     ├── operations/        (deployment, monitoring, runbooks, incident-response — Tier 3+)
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
   - **Operational capture (Tier 3+):** after a deploy, a production-incident debug, or an outage, record the playbook in `docs/operations/runbooks.md` (detection signal → first response → diagnosis → remediation → escalation), and note post-incident learnings in `docs/operations/incident-response.md`. Runbooks are **designed to accrete from real operational work** — capture the entry in the same session you did the ops work; don't defer it to a cold backfill pass.

3. **Documentation philosophy (enforced)**
   - Living, minimal, high-signal only.
   - One source of truth per topic.
   - No duplicate information between files.
   - Update immediately when the project evolves — never let docs drift from reality.
   - Goal: A new senior developer could be fully productive in <2 hours just by reading `/docs`.

### Canonical References (portfolio-wide standards)

Do NOT copy canonical documents into this project. Instead, read them from the hub:
- **Universal Web Development Principles**: `C:/dev/project-hub/canonical/references/Universal_Web_Development_Principles.md`
- **Versioning Standards**: `C:/dev/project-hub/canonical/references/VERSIONING_STANDARDS.md`

These are the single source of truth. If you need to reference a principle, point to the canonical path — never create a local copy.

### How this interacts with hub sync

- This `Docs-First Rule` block is **hub-canonical** — `hub sync` may re-insert it if removed.
- Everything else in this `CLAUDE.md` is **project-owned** — hub will never overwrite it.
- If hub updates this block, accept the update. It will never conflict with project-specific content.
```

---

## Step 1b: Session Continuity block (Tier 3+ only)

**This step applies ONLY to Tier 3 and Tier 4 projects.** Determine the tier from `docs/architecture/PROJECT_CLASSIFICATION.md` (or the hub registry). **If the project is Tier 1 or Tier 2, or the tier cannot be determined, SKIP this step entirely** — the session-continuity handoff is scoped to projects with active multi-session development (see `canonical/standards/session-continuity.md`). Putting it on a static/marketing site is bloat.

For Tier 3+ projects:

- Check whether `CLAUDE.md` already contains the exact heading `## Session Continuity — READ FIRST`.
  - If yes → do nothing (don't duplicate).
  - If no → insert the block below **immediately after the `Docs-First Rule` block** from Step 1 (or, if that block is absent, after the first `# `/`## ` heading).
- **Do NOT create `docs/planning/NEXT_SESSION.md` in this step.** It is created lazily on first real use (see the block text). Pre-stamping an empty handoff doc is bloat and would defeat the staleness check.

### The block to insert (do not alter any wording):

```
## Session Continuity — READ FIRST

At the start of every session, read `docs/planning/NEXT_SESSION.md` before doing anything else — it carries the paste-ready handoff prompt and the current open follow-ups. If that file does not exist yet, create it at the end of your first session that lands material work, using the template in `C:/dev/project-hub/canonical/standards/session-continuity.md`. Update it at the end of every session that lands material work (on `main`, or on the active feature/integration branch when work-in-flight hasn't merged yet). If it is stale — its last-updated date is older than the most recent commit on the branch it describes — say so and update it as part of the session.

*(This block is hub-owned and Tier 3+ only; `hub sync` may re-insert it. Everything else in this CLAUDE.md is project-owned.)*
```

### How this interacts with hub sync

- This `Session Continuity` block is **hub-canonical for Tier 3+** — `hub sync` may re-insert it if removed (Tier 3+ projects only; Tier 1–2 never receive it).
- The `NEXT_SESSION.md` file it points to is **project-owned and never synced** — its entire value is the project's real, current state, which only a session in that repo can author. Sync carries the pointer; the repo grows the content.

---

## Step 2: Create docs/docs-maintenance.md (only if missing)

Create the file at exactly `docs/docs-maintenance.md` with this exact content:

```markdown
# Docs Maintenance Rules (for AI + Humans)

These rules are binding for every Claude Code session and every code change in this project.

## Always
- Read the entire /docs folder before starting any task.
- Update docs only when the change matters for a future developer or team handoff.
- Prefer editing existing files over creating new ones.
- Keep additions concise — prefer <10 lines when possible.
- Use tables, lists, and diagrams only when they reduce total words.
- Flag documentation drift explicitly: "Documentation update required: [file] — [what changed]"

## Never
- Add tutorial-style fluff, repetition, or overview sections that already exist elsewhere.
- Create a new subfolder unless a genuinely new category appears (e.g., new `security/` or `deployment/` concern with 3+ related files).
- Let docs get out of sync with code — if you changed it, update the doc in the same session.
- Delete history — use `[DEPRECATED]` markers or move to `docs/archive/`.

## Approved top-level structure (do not add folders without explicit approval)
- `architecture/`   — system design, data models, diagrams
- `adr/`            — Architecture Decision Records (`adr-NNN-*.md`; Tier 1+)
- `examples/`       — working code samples, patterns, usage demos
- `operations/`     — deployment, monitoring, runbooks, incident-response (Tier 3+)
- `planning/`       — roadmaps, milestones, feature specs in progress
- `reference/`      — API docs, config schemas, environment variables, quick-ref tables
- `specifications/` — finalized feature specs, acceptance criteria
- `archive/`        — deprecated content (never delete, just move here)
- `docs-maintenance.md` — this file

## File naming
- Lowercase, hyphen-separated: `auth-flow.md`, not `AuthFlow.md`
- Be specific: `supabase-rls-patterns.md`, not `database.md`
- ADRs: `adr-001-framework-choice.md` (zero-padded number + slug)

## When to create a new file vs. edit an existing one
- Edit existing: adding a section, updating a pattern, correcting outdated info
- New file: entirely new topic with no existing home, OR existing file exceeds ~200 lines and has a clean split point
- When in doubt: edit existing
```

---

## Step 3: Backfill the /docs folder

Using this `CLAUDE.md`, the existing codebase, and any project context visible in the session, populate the `/docs` folder with meaningful, accurate documentation.

**Backfill priorities (in order):**

1. **`docs/architecture/`** — What is this project? What are its key systems, data flows, and deployment model? Include a brief system overview and any non-obvious architectural decisions already made.

2. **`docs/specifications/`** — What does this project do? What are the core features, user flows, and acceptance criteria for work that already exists?

3. **`docs/reference/`** — What does a developer need to get started? Environment variables, key commands, config schema, external service dependencies. **Do not copy canonical hub documents** (Universal Web Dev Principles, Versioning Standards) into this folder — those live in `project-hub/canonical/references/` and are referenced by path in CLAUDE.md.

4. **`docs/planning/`** — What's next? If there's an active roadmap or known next steps, document them. Even a simple `roadmap.md` with 3-5 bullet points is better than nothing.

5. **`docs/examples/`** — Are there patterns, conventions, or non-obvious code approaches in this project that a new developer would need to know? Document 1-3 of the most important ones.

6. **`docs/operations/`** (Tier 3+ only) — deployment steps and monitoring/alerting signals. For **`runbooks.md`** (Tier 4) and **`incident-response.md`** (Tier 4), a structured skeleton keyed to *this* project's real detection signals is an acceptable start — unlike other docs, these are designed to fill in from real incidents (see the Operational-capture rule in CLAUDE.md), so seeding the shape is not placeholder-stub bloat.

**Backfill rules:**
- Only create files you can populate with real, accurate content — no placeholder stubs. (Exception: Tier-4 `docs/operations/runbooks.md` / `incident-response.md` may start as a signal-keyed skeleton — see priority 6.)
- Infer from the codebase, not from imagination.
- If you're uncertain about something, note the uncertainty inline: `[VERIFY: is this still the deployment target?]`
- Keep each file scannable — use headers, short paragraphs, and tables.

---

## After completing all steps

Output:
1. A clean tree view of the `/docs` folder (including all new/updated files)
2. A summary of exactly what was created or modified in `CLAUDE.md`, `docs/docs-maintenance.md`, and `/docs/`
3. The 2-3 most important docs files for the project owner to review and expand next, with a one-sentence reason for each

---

## Hub Integration Notes

This command is hub-canonical and synced to all Tier 2+ projects via `hub sync`.

- **Safe to re-run** — idempotent. Re-running after docs exist will only add missing sections.
- **Tier targeting**: Sync to Tier 2+ projects. Tier 1 (static/marketing) gets a lighter stub: just the `Docs-First Rule` block in CLAUDE.md and a minimal `docs/reference/` with setup instructions.
- **CLAUDE.md ownership**: Hub owns only the `Docs-First Rule` block. All other content in CLAUDE.md is project-owned and will never be overwritten by `hub sync`.
- **When to run**: Once per project during initial retrofit (`hub retrofit`), or on-demand when a project's docs are missing or stale.

---

## Gate: Canonical Pattern Validation Standard (All-Source Fusion)

**Before any practice, pattern, or workflow step enters canonical**, the following confidence standard applies:

| Condition | Confidence Level | Action |
|-----------|:----------------:|--------|
| Derived from 1 project | `low` | Enter with `confidence: low`. Mandatory note: "Requires 3+ independent project validations before treatment as established standard." |
| Validated in 2 projects | `moderate` | Update confidence. Not yet established — cannot be cited as "the hub standard." |
| Validated in 3+ projects | `high` | Update confidence. Can be cited as a validated hub standard. |
| Validated in 5+ projects with zero exceptions | `established` | Update confidence. Treatment as canonical standard is fully warranted. |

**Enforcement:** When using `hub retrofit` to encode a new canonical practice, the retrofit change record must include the current `validation_count` and `confidence` level. A practice at `confidence: low` must carry a prominent warning in the canonical file frontmatter that it is not yet a validated standard.

**False Corroboration check:** When counting `validation_count`, only independent projects count. Projects that copied the pattern from each other (via hub sync) do not each count as independent validations — they count as one validation of the originating project.

---

## Gate: Rollback Path Pre-Propagation Requirement

**Before any retrofit propagates to registered projects**, a rollback path must be documented. This is a pre-propagation gate, not a post-hoc step. A retrofit without a documented rollback path must not be propagated.

Add an entry to `registry/retrofits.json` before propagating:

```json
{
  "retrofit_id": "RET-YYYY-NNNN",
  "date": "YYYY-MM-DD",
  "canonical_file": "canonical/commands/example.md",
  "change_description": "What changed and why",
  "projects_affected": 0,
  "rollback_documented": true,
  "rollback_path": "Exact steps to revert this change in all affected projects",
  "outcome_review_date": "YYYY-MM-DD (30 days after propagation)",
  "outcome_review": null
}
```

**Rollback path requirements:**
- Must be specific enough that someone unfamiliar with the change could execute the revert
- Must identify the affected files in each project
- Must note whether rollback requires a follow-up `hub sync` or can be done per-project

**Outcome review:** 30 days after propagation, the dashboard surfaces the retrofit for outcome review (separate from individual decision reviews).

---

## Gate: Brooks's Law — Subtraction First

**Before any new canonical addition is proposed**, the first question must be: *"What can be removed or simplified from the canonical layer to make room for this?"*

This is not a form — it requires a written answer. The subtraction check is recorded in the `hub-retrofit` interactive gate (Gate A) and echoed in the retrofit report.

**Enforcement:** `bin/hub-retrofit` prompts for a subtraction check before Step 1. The answer (or explicit acknowledgment that nothing can be removed) is required before proceeding.

---

## Gate: Cargo Cult Mechanism Check

**Before encoding any new canonical practice**, document:

1. **Mechanism designed to activate:** What is this practice supposed to do — what behavior does it produce?
2. **Evidence mechanism is running:** How do we know it's actually producing that outcome (not just present in documentation)?
3. **Cargo cult risk:** `low` | `medium` | `high` — is this practice producing the intended outcome, or is it theater?

**Enforcement:** `bin/hub-retrofit` prompts for the mechanism check (Gate B) before Step 1. This gate is advisory — it does not block the retrofit, but skipping it without acknowledgment is a failure of epistemic duty.

**Note:** This gate is advisory in the scout agent output (`mechanism_check` field in recommendations is a prompt to the reviewer). The retrofit gate is mandatory; the scout field is advisory.
