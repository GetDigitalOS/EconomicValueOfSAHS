# Versioning Standards

| Field | Value |
|-------|-------|
| **Version** | v1.01.01 |
| **Date** | 2026-03-15 |
| **Status** | Locked |
| **Applies To** | All hub documents, project documents, and canonical reference files |

### Changelog

| Version | Date | Changes |
|---------|------|---------|
| v1.01.01 | 2026-03-15 | First locked version. Captures versioning schema, changelog format, status lifecycle, and document header standard in use across hub documents. |

---

## 1. Version Schema

All documents follow a three-part version number:

```
v[MAJOR].[MINOR].[PATCH]
```

### Draft vs. Locked

- **Draft documents** use a `0` major version: `v0.xx.xx`
- **First locked version** is always `v1.01.01` — not `v1.0.0`
- Once locked, documents increment from `v1.01.01`

### Increment Rules

| Segment | Increment When |
|---------|----------------|
| **MAJOR** | New sections added, structural reorganization, breaking changes to document contract |
| **MINOR** | Individual entries added, sections expanded, meaningful content additions |
| **PATCH** | Wording edits, typo fixes, clarifications that do not change meaning |

### Formatting Rules

- Minor and patch segments are always **two digits**: `01`, `02` ... `09`, `10`
- Example progression: `v0.01.01` → `v0.01.02` → `v0.02.01` → `v1.01.01` → `v1.01.02`
- Do **not** change the version number when only updating the `Status` field

---

## 2. Status Lifecycle

Every document carries a **Status** field in its header. Valid values:

| Status | Meaning |
|--------|---------|
| `Draft` | Work in progress. Content unstable. Not yet a decision-making reference. |
| `Review` | Content complete. Under active review before locking. |
| `Locked` | Approved as canonical. Changes require a version increment and changelog entry. |
| `Deprecated` | Superseded by another document. Retained for audit trail only. |
| `Archived` | No longer active. Preserved for historical reference. |

**Rule:** Status and Version are independent. Do not bump the version number to reflect a status change only. Record the status change in the changelog.

---

## 3. Document Header Standard

Every versioned document must open with this metadata table:

```markdown
| Field | Value |
|-------|-------|
| **Version** | v0.01.01 |
| **Date** | YYYY-MM-DD |
| **Status** | Draft |
```

Optional fields (include when relevant):

```markdown
| **Applies To** | [scope description] |
| **Owner** | [person or role] |
| **Supersedes** | [prior document name/version] |
```

The header table appears **before** any prose or section content, immediately after the document `# Title`.

---

## 4. Changelog Format

Every versioned document must include a changelog section. Placement: **immediately after the header table**, before body content.

```markdown
### Changelog

| Version | Date | Changes |
|---------|------|---------|
| v0.01.01 | YYYY-MM-DD | Initial draft. |
```

### Changelog Entry Rules

- Entries are listed **newest first** (descending)
- Each entry must describe *what changed and why*, not just what was touched
- Status transitions are recorded in the changelog even when version does not increment
- Entry removals (for documents like `wisdom_synthesis.md`) are as informative as additions — record what was removed and why
- Entries should be self-contained enough to understand the change without reading a diff

### Entry Quality Standard

| Poor Entry | Good Entry |
|------------|------------|
| `Updated Section 3` | `Expanded Section 3: added two new failure mode examples for async decision queues` |
| `Minor edits` | `Clarified outcome review trigger conditions; no change to decision logic` |
| `Draft → Locked` | `First locked version. Status: Draft → Locked. No content changes from v0.02.03.` |

---

## 5. Canonical File Versioning (Commands, Agents, Workflows)

Canonical files distributed by `hub sync` use YAML frontmatter. These files include a `version` field in their frontmatter rather than a markdown header table:

```yaml
---
version: "v1.01.01"
owner: "@getdigital2020"
# ... other frontmatter fields
---
```

The same increment rules (Section 1) apply. When hub-sync distributes a canonical file, the version in the frontmatter tells each project exactly which version it has.

---

## 6. README Files

README files (`README.md`) are **folder scaffolding**, not versioned documents. They do not require a version header, changelog, or status field. They describe the contents and purpose of a directory and are updated in place without version tracking.

---

## 7. Applying This Standard to a New Document

When creating a new versioned document:

1. Add the header table immediately after the `# Title`
2. Set version to `v0.01.01`, date to today, status to `Draft`
3. Add a changelog section with a single entry: `v0.01.01 | [date] | Initial draft.`
4. Write content
5. Increment version and add a changelog entry for each meaningful change
6. When content is approved as canonical: set status to `Locked`, set version to `v1.01.01`, record in changelog

---

## 8. Scope and Applicability

This standard applies to:

- All hub canonical documents (framework files, reference files, protocol documents)
- All project-level documentation that will be cited in hub decisions
- Any document described as "canonical," "locked," or referenced by a hub command
- Canonical files (commands, agents) via YAML frontmatter `version` field

This standard does **not** apply to:

- `README.md` files (folder scaffolding — see Section 6)
- Ephemeral working notes or scratch documents
- Code files (use semantic versioning via `package.json` or equivalent)
- External documents not maintained within the hub

---

*This document is itself governed by the standard it defines.*
