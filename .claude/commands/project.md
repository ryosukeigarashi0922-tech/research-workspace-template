---
description: "Load project context, show status dashboard and suggest next actions"
argument-hint: "[project-name]"
---

# Project: Project Launcher

Receive a project name, load context, and present current status with available actions.

Target project: $ARGUMENTS (select from list if not specified)

---

## Phase 1: Project Identification

### If $ARGUMENTS is specified

1. Search in this order:
   - `flow/<project>/README.md`
   - `stock/<project>/README.md`
   - Partial match: directories under flow/ and stock/ containing $ARGUMENTS in name
2. If multiple matches: ask user to select
3. If not found:
   - Report "Project '$ARGUMENTS' not found"
   - Suggest "Create with `/new-project $ARGUMENTS`?" and exit

### If $ARGUMENTS is not specified

1. List project directories under flow/ and stock/ (those with README.md)
2. Display location of each project:

```
Available projects:
  flow/project-alpha/     — [1-line summary from README section 1]
  stock/project-beta/     — [1-line summary from README overview]
```

3. Ask user to select

## Phase 2: Context Loading

Once project is identified, load the following in parallel:

1. **README.md** — Read thoroughly (both flow/ and stock/ if both exist)
   - Section 1: Purpose, Section 3: Scope, Section 6: Deliverables, Section 9: Current focus
2. **File inventory** — Collect all files
   - All files in `flow/<project>/` (if exists)
   - All files in `stock/<project>/` (if exists)
3. **literature-review.md** — Check existence (if present, read summary section)
4. **research-plan.md** — Check existence (if present, read current wave and status)

## Phase 3: Status Dashboard

Compose and display the following dashboard from collected information:

```
## <project-name>

**Purpose**: [2-3 sentences from README section 1]
**Scope**: [Key points from README section 3]
**Status**: [Status from README metadata]

### File Composition
| Location | File | Type | Summary |
|----------|------|------|---------|
| flow | dr-topic.md | DR output | Topic research |
| flow | analysis.md | Analysis | Interim analysis |
| stock | deliverable.md | Deliverable | Final output |

### Progress Indicators
- Literature Review: [Available (last updated: YYYY-MM-DD) / None]
- Research Plan: [Wave N (pending X / done Y) / None]
- flow files: X, stock files: Y

### Current Focus (README section 9)
[Display section 9 content as-is]
```

## Phase 4: Available Actions

Suggest executable commands based on project state:

### Decision Logic

| Condition | Suggested Action |
|-----------|-----------------|
| literature-review.md does not exist | `/expand <project>` — Run Literature Review + gap analysis |
| literature-review.md has lifecycle recommendations | `/curate <project>` — Execute file organization based on recommendations |
| flow/ has reviewed files | `/promote <project>` — Graduate deliverables to stock/ |
| flow/ has unnecessary files | `/archive` — Move unnecessary files to archive/ |
| research-plan.md has pending items | "N pending research items awaiting DR execution" |
| Neither flow/ nor stock/ exists | `/new-project <project>` — Initialize project |

### Display Format

```
### Next Actions
1. `/expand <project>` — Literature Review has not been conducted yet
2. Research item Q2-3 (DR, high priority) is pending — see research-plan.md
3. `/promote <project>` — Files ready for stock promotion
```

If additional work is requested, respond using the already-loaded context.
