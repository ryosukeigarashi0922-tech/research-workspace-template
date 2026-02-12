---
description: "Execute ready items from research-plan.md backlog via agent team"
argument-hint: "[project-name]"
---

# Run: Backlog Execution Engine

Select ready items from research-plan.md backlog and execute them via agent team.

Output:
- Updated `flow/<project>/research-plan.md` — status changes + Execution Log entry
- Generated research output files (dr-*.md, lit-*.md, etc.)

Pipeline position:
```
/expand → research-plan.md (backlog) → /run (execute) → /expand (review + add items)
```

Target project: $ARGUMENTS (if unspecified, ask)

---

## Phase 1: Backlog Analysis

**Actor**: lead (main agent)

1. Resolve project: find `flow/<project>/research-plan.md`
   - If not found: "No research-plan.md found. Run `/expand <project>` first." → stop
2. Read research-plan.md
3. **Migration check**: detect old Wave format
   - Heuristic: header contains `Current wave:` OR IDs match `Q\d+-\d+` pattern OR sections match `## Wave \d+`
   - If old format detected → execute Migration Procedure (see below) → continue with migrated data
4. Parse Backlog section:
   - Extract all items with status `pending` or `blocked`
   - Build dependency graph from Dependencies section
5. Determine ready items:
   - `ready` = `pending` AND (no dependencies OR all dependencies `done`)
   - `blocked` = `pending` AND has unresolved dependencies
6. Sort ready items by priority: High > Medium > Low, then by Q-number
7. If 0 ready items:
   - All done → "All backlog items are complete. Run `/expand` to identify new gaps."
   - All blocked → "N items blocked. Unresolved dependencies: [list]. Complete blocking items first."
   - Stop

---

## Phase 2: Execution Plan Presentation

**Actor**: lead

1. Classify ready items by method and estimate costs:

| Method | Count | Est. cost | Est. time |
|--------|-------|-----------|-----------|
| DR (o4-mini) | N | ~$X | N×10min |
| DR (o3) | N | ~$X | N×15min |
| search-scholar | N | $0 | instant |
| WebSearch | N | $0 | instant |
| Analysis | N | $0 | N×30-60min |
| Manual | N | — | user-dependent |
| **Total** | **M** | **~$Y** | **~Z** |

2. Propose recommended batch:
   - All high-priority ready items
   - If >8 items, split into recommended batch (top 5-8) + remaining
3. Present via AskUserQuestion:

| Option | Behavior |
|--------|----------|
| Recommended batch (default) | Execute high-priority ready items |
| All ready items | Execute all N ready items |
| Top N items | Execute first N items by priority |
| Custom selection | User specifies Q-IDs (e.g., Q-1, Q-3, Q-7) |
| Cancel | Stop without execution |

4. For any DR o3 items in selection: individual confirmation required

---

## Phase 3: Team Execution

**Actor**: lead + execution agents

### Small batch (< 3 automated items)

Lead executes directly without team creation.

### Team execution (≥ 3 automated items)

1. TeamCreate `run-<project>`
2. Dynamic team composition based on selected items:

| Role | subagent_type | Condition | Responsibility |
|------|--------------|-----------|----------------|
| **dr-runner-1..N** | `general-purpose` | DR items exist | deep_research MCP → `dr-*.md`. 1-3 agents (target: 2 items/agent) |
| **scholar** | `general-purpose` | Literature items exist | search-scholar MCP → `lit-*.md` |
| **analyst** | `general-purpose` | Analysis items exist | Existing file synthesis → `*.md` |

3. Dependency-aware sub-batch execution:
   - Sub-batch A (independent items) → all complete → Sub-batch B (depends on A) → ...
   - Between sub-batches: present summary to user, confirm continuation

4. Manual items: not auto-executed. Present guidance:
   - Required information, contacts, suggested questions
   - Mark as `blocked: awaiting human input` in backlog

### Task Instruction Templates

#### dr-runner

```
subject: "DR execution: Q-X, Q-Y"
description:
  Execute Deep Research for the following items.

  ## Q-X: [item name]
  - Purpose: [1-2 sentences]
  - Prompt: [English prompt from DR Prompts section]
  - Model: [o4-mini / o3]
  - Save to: [output path]

  ## Execution steps (per item)
  1. Load deep_research MCP tool (ToolSearch "deep_research")
  2. Execute prompt via deep_research tool
  3. Write result to save path
  4. git add <save path>
  5. SendMessage to lead with completion report
```

#### scholar

```
subject: "Literature search: Q-X"
description:
  Execute academic literature search.

  ## Q-X: [item name]
  - Purpose: [1-2 sentences]
  - Search keywords: [English keywords]
  - Save to: [output path]

  ## Execution steps
  1. Load search-scholar MCP (ToolSearch "search-scholar")
  2. Search for academic literature
  3. Organize key paper abstracts and findings
  4. Write result to save path
  5. git add <save path>
  6. SendMessage to lead with completion report
```

#### analyst

```
subject: "Synthesis analysis: Q-X"
description:
  Execute synthesis analysis of existing files.

  ## Q-X: [item name]
  - Purpose: [1-2 sentences]
  - Input files: [list of file paths]
  - Save to: [output path]

  ## Execution steps
  1. Read all input files
  2. Perform synthesis analysis aligned with research purpose
  3. Write result to save path
  4. git add <save path>
  5. SendMessage to lead with completion report
```

---

## Phase 4: Wrap-Up

**Actor**: lead

1. **Dissolve team** (if Phase 3 used team):
   - shutdown_request to all teammates → TeamDelete `run-<project>`
2. **Update research-plan.md**:
   - Change executed items: `pending` → `done`
   - Failed items: `pending` → `failed`
   - Append Execution Log entry:
     ```
     ### Run N (YYYY-MM-DD)
     - **Executed**: Q-X, Q-Y, Q-Z
     - **Results**: Q-X done, Q-Y done, Q-Z failed (reason)
     - **Cost**: ~$N (method breakdown)
     ```
   - Recalculate coverage assessment if applicable
3. **Update README.md** section 8 (file inventory), section 9 (next actions)
4. `git add` all changed files individually
5. **Present result summary**:
   - Executed items and outcomes
   - Remaining backlog: pending N / done N / blocked N / failed N
   - Recommended next steps:
     - More ready items exist: "Run `/run <project>` again for next batch"
     - Blocked items: "Complete [manual items/dependencies], then `/run` again"
     - All done: "Run `/expand <project>` to review results and identify new gaps"
   - "Changes are not committed. Run `git commit` after review."

---

## Migration Procedure (Wave → Backlog)

Triggered when old Wave format is detected in Phase 1.

### Detection Heuristics

Any of the following indicates old Wave format:
- Header contains `Current wave: N`
- Item IDs match `Q\d+-\d+` pattern (e.g., Q1-1, Q2-3)
- Section headers match `## Wave \d+`

### Conversion Steps

1. Parse all Wave sections, collecting all items
2. Aggregate into a single flat list
3. Renumber with global sequential IDs:
   - Order: Wave number → priority (High→Medium→Low) → original number
4. Create ID mapping table (Q1-1 → Q-1 etc.)
5. Rewrite dependency references using new IDs
6. Rewrite DR Prompt headers using new IDs
7. Add metadata columns:
   - `Added`: Wave header date
   - `Source`: Wave 0 items → `initial`, Wave 1+ items → `expand`
8. Replace emoji statuses with text:
   - ⏳ → pending
   - ✅ → done
   - 🔲 → blocked
   - ❌ → failed
9. Remove `Current wave: N` header
10. Replace `## Wave N` sections with `## Backlog` + priority subsections
11. Add empty `## Execution Log` section
12. Append migration note:
    ```
    > **Migration note** (YYYY-MM-DD): Converted from Wave format. ID mapping: [key mappings]
    ```

### User Notification

Present migration summary before continuing:
- Number of items migrated
- ID mapping table (abbreviated if >10 items)
- Any issues found during migration

---

## Edge Cases

| Case | Response |
|------|----------|
| No research-plan.md | Direct to `/expand` |
| All items done | Direct to `/expand` |
| All items blocked | Show dependency graph, suggest manual resolution |
| DR MCP not connected | Fall back to WebSearch + save DR prompts for later |
| DR fails or low quality | Mark `failed`, propose re-execution with o3 |
| Manual items only | Present guidance, no team needed |
| Cost exceeds tolerance | Triage by priority, execute within budget |

---

## Tools and Plugins Used

| Phase | Tool/Plugin | Agent | Purpose |
|-------|------------|-------|---------|
| Phase 1 | Read, Grep | lead | Parse research-plan.md |
| Phase 3 | deep_research MCP | dr-runner | Deep Research execution |
| Phase 3 | search-scholar MCP | scholar | Academic literature search |
| Phase 3 | Read, Write | analyst | Existing file synthesis |
| Phase 4 | Edit, Write | lead | Update research-plan.md, README.md |
