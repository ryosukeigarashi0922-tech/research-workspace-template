---
description: "Literature Review + gap analysis + research planning for a project (generates literature-review.md + research-plan.md)"
argument-hint: "[project-name]"
---

# Expand: Literature Review + Gap Analysis + Research Planning

Read all project files (flow/ + stock/), conduct a Literature Review evaluating content and quality,
assess coverage against project scope, and develop a research plan for gaps.

Output:
- `flow/<project>/literature-review.md` — Per-file evaluation + lifecycle recommendations
- `flow/<project>/research-plan.md` — Cumulative research plan (Wave management)

Target project: $ARGUMENTS (ask if not specified)

---

## Agent Organization

Switch execution mode based on number of target files.

### Simple Mode (5 files or fewer)

Main agent executes all phases directly. Efficient through parallel reads, no team formation.

### Team Mode (6+ files)

Create team with TeamCreate as `expand-<project>` and coordinate Phases 2-4.

#### Team Composition

| Role | subagent_type | Responsibility | Notes |
|------|--------------|----------------|-------|
| **lead** (self) | — | Phase 1, integration analysis, user dialogue, Phase 5-6 | Team leader |
| **reviewer-N** | `Explore` | Phase 2: File reading (2-3 files/agent) | Read-only. 2-5 agents depending on file count |
| **cross-analyst** | `Explore` | Phase 2: Cross-file analysis | Receives all review results, detects duplicates/contradictions/gaps |
| **mece-validator** | `Explore` | Phase 3: MECE decomposition validation | Checks for gaps, overlaps, granularity |
| **brainstormer** | `general-purpose` | Phase 4: Divergent research item exploration | Uses scientific-brainstorming skill |

#### Task Dependencies

```
review-batch-1 ──┐
review-batch-2 ──┼── cross-file-analysis ── mece-validation ── brainstorming
review-batch-N ──┘
                       ^                      ^                  ^
                  (after all reviews)    (after user OK)    (after MECE confirmed)
```

**Note**: Due to user confirmation gates, tasks are added per Phase, not all at once.

#### Team Lifecycle

1. After Phase 1 completion, if 6+ files, TeamCreate `expand-<project>`
2. TaskCreate for Phases 2-4 as each Phase progresses
3. Add next Phase tasks after user confirmation at each gate
4. After Phase 4 completion, shutdown_request to all teammates -> TeamDelete

---

## Phase 1: Current State Assessment

**Executor**: lead (main agent)

1. Read `flow/<project>/README.md`
   - Section 1: Purpose, Section 3: Scope, Section 6: Deliverables, Section 7: Success Criteria, Section 8: File inventory, Section 9: Current focus
   - If no README.md: suggest "`/new-project <project>` to create README first" and exit
2. Collect target files (exclude README.md, literature-review.md, research-plan.md):
   - All files in `flow/<project>/`
   - All files in `stock/<project>/` (if exists)
3. Check `flow/<project>/research-plan.md`:
   - **Exists**: Read previous Wave number & item statuses -> **incremental update mode**
   - **Doesn't exist**: **new creation mode** (start from Wave 1)
4. Check `flow/<project>/literature-review.md`:
   - **Exists**: Load previous review, re-review only changed files
   - **Doesn't exist**: Full review of all files
5. If 6+ files: TeamCreate `expand-<project>`

## Phase 2: Literature Review (Per-file Reading)

Read each file collected in Phase 1 and evaluate on the following dimensions.

### Simple Mode

Main agent reads all files via parallel Read, evaluates, and conducts cross-file analysis.

### Team Mode

#### Step 2a: Parallel File Reviews

Assign tasks to each reviewer-N for parallel execution.

Each reviewer TaskCreate content:
- **subject**: `Review: file1.md, file2.md`
- **description**: Use the prompt template below

```
Read and evaluate the assigned files against the following project scope.

## Project Purpose (from README section 1)
[paste purpose]

## Project Scope (from README section 3)
[paste scope]

## Files to Evaluate
- flow/<project>/file1.md
- flow/<project>/file2.md

## Evaluation Criteria (answer for each file)
1. Summary (3-5 sentences)
2. Coverage area (which scope areas does it address)
3. Source quality (primary source / secondary source / DR raw output / author analysis / speculation)
4. Depth (overview level / practical analysis / systematic research)
5. Currency (information date and update necessity)
6. Limitations/gaps (important aspects not covered)
7. Cross-file relationship clues (specific quotes of suspected duplicates, references, prerequisite relationships)
8. Lifecycle recommendation (maintain / stock promotion candidate / archive candidate / consolidation candidate / needs update) with rationale

Record specific facts, figures, and claims in citation format (used for cross-file analysis).
```

After reviewer completion, send review results to lead via SendMessage.

#### Step 2b: Cross-file Analysis

After all reviewers complete, assign task to cross-analyst:

```
Analyze the relationships between files based on individual review results below.

## Individual Review Results
[paste all reviewer results]

## Analysis Dimensions
1. **Duplicates**: File pairs covering the same area/topic (point out specific overlap)
2. **Contradictions**: Different facts/figures/claims between files (cite both for comparison)
3. **Complementary relationships**: File combinations that improve coverage together
4. **Gaps**: Scope areas covered by no file
5. **Consolidation candidates**: File groups with high content overlap that should be merged
```

#### Step 2c: Lead Integration

Receive cross-analyst results and compile all review results into literature-review.md draft format.

### Per-file Evaluation Criteria

| Dimension | Description |
|-----------|-------------|
| **Summary** | Content overview (3-5 sentences) |
| **Coverage area** | Which scope areas from README section 3 it addresses |
| **Source quality** | Primary source / secondary source / DR raw output / author analysis / speculation |
| **Depth** | Overview level / practical analysis / systematic research |
| **Currency** | Information date (current / somewhat dated / needs update) |
| **Limitations/gaps** | Important aspects not covered by this document |
| **Cross-file relationships** | Duplicate / complementary / contradictory / prerequisite |
| **Lifecycle recommendation** | Maintain / stock promotion candidate / archive candidate / consolidation candidate / needs update |

### Incremental Update Behavior

When `literature-review.md` already exists:
1. Re-review only files changed/added since last review (limit reviewer assignments to changed files)
2. Mark entries for deleted files as "(deleted)"
3. Maintain previous evaluation for unchanged files
4. Re-evaluate cross-file analysis in full (due to new-old mix)

### User Confirmation Gate

Present Literature Review results to user and confirm:
- Recognition corrections for each file's evaluation
- Agreement/modifications to lifecycle recommendations (actual moves done separately via `/promote` `/archive`)
- **Get approval before proceeding to Phase 3**

## Phase 3: Coverage Analysis (MECE)

Based on Phase 2 Literature Review, evaluate coverage against README section 3 (Scope).

### Analysis Steps

1. **Lead creates initial MECE decomposition**
   - If A/B/C structure already exists, adopt it
   - Otherwise, decompose into 3-6 areas based on content

2. **mece-validator verification** (Team Mode only)

   Task for mece-validator:
   ```
   Verify the following MECE decomposition proposal.

   ## Project Scope (README section 3)
   [scope]

   ## Deliverables Definition (README section 6)
   [deliverables]

   ## Success Criteria (README section 7)
   [success criteria]

   ## Coverage Status from Literature Review
   [Phase 2 cross-file analysis summary]

   ## MECE Decomposition Proposal
   [area list]

   Verification criteria:
   1. Is every scope element covered by at least one area? (gap check)
   2. Are there items with ambiguous boundaries between areas? (overlap check)
   3. Is the granularity balance between areas appropriate?
   4. Can each deliverable/success criterion be mapped to an area?
   5. Are there overlooked implicit areas? (prerequisite knowledge outside scope but needed for deliverables)

   If corrections needed, propose specific modifications.
   ```

3. **Apply verification results and finalize decomposition**

4. For each area, integrate Literature Review results and evaluate:
   - **Coverage level**: High (integrated analysis done) / Medium (DR output exists, not integrated) / Low (not started)
   - **Supporting files**: Which files provide coverage (from both flow/stock)
   - **Quality assessment**: DR raw output only / analyzed / reviewed
   - **Quality rationale**: Derived from Literature Review depth/source evaluation

### User Confirmation Gate

Present coverage table to user and confirm:
- In incremental update mode, also show changes from previous assessment
- Incorporate user feedback (area additions/merges/recognition corrections)
- **Get approval before proceeding to Phase 4**

## Phase 4: Research Item Design

For areas with "Medium"/"Low" coverage and file gaps identified as "needs update"/"has limitations" in Literature Review:

### Research Item Development

**Lead** derives research items logically from coverage gaps (deductive approach).
Simultaneously, **brainstormer** conducts divergent exploration in parallel (Team Mode).

Task for brainstormer:
```
Using the scientific-brainstorming skill, brainstorm research questions
for the following project's coverage gaps.

## Project Purpose
[README section 1]

## Project Scope
[README section 3]

## Coverage Gaps (MECE analysis results)
[Low/Medium area list and existing file limitations/gaps]

## Research items already derived by lead (for avoiding duplicates)
[lead's deductive list]

## Constraints
- Methods: DR (Deep Research) / Analysis (existing file integration) / Notion / Manual / Literature
- Each item should be expressed as one clear question

Propose research items from angles not typically reached by logical deduction:
oblique perspectives, interdisciplinary viewpoints, aspects practitioners tend to overlook.
Add a 1-sentence reason for why each angle matters.
```

**Merge and deduplicate** lead's deductive list and brainstormer's inductive list.

### Per-item Format

1. **Develop specific Research Questions**
   - 1 item = 1 clear question
   - Attach to each item:
     - **Method**: `DR` (Deep Research) / `Analysis` (existing file integration) / `Notion` / `Manual` (human input) / `Literature`
     - **Priority**: High / Medium / Low (impact on deliverables x execution feasibility)
     - **Estimated time**: DR=5-10min, Analysis=30-60min, etc.
     - **Dependencies**: Does it require results from other items?

2. **Attach prompt drafts for DR items** (per CLAUDE.md Deep Research integration rules)
   - Research objective (1-2 sentences)
   - English prompt (ready to paste into Deep Research)
   - Expected output
   - Save path: `flow/<project>/dr-<topic>.md`

3. **Present all items to user for additions, modifications, deletions**

## Phase 5: Output

### Team Mode: Disband Team

After Phase 4 completion, send shutdown_request to all teammates, TeamDelete `expand-<project>`.
Lead directly executes remaining output work.

### literature-review.md

```markdown
# Literature Review
> Project: <project-name>
> Last updated: YYYY-MM-DD
> Files reviewed: N (flow: X, stock: Y)

## Review Summary
- Total files: N
- Quality distribution: Systematic research X / Practical analysis Y / DR raw output Z
- Lifecycle recommendations: Stock promotion candidates X / Archive candidates Y / Consolidation candidates Z

## Per-file Reviews

### flow/<project>/

#### <filename>.md
- **Summary**: ...
- **Coverage area**: A. xxx, B. xxx
- **Source quality**: DR raw output (Deep Research)
- **Depth**: Overview level
- **Currency**: 2026-02 (current)
- **Limitations/gaps**: ...
- **Cross-file relationships**: Overlaps with dr-yyy.md in area A
- **Lifecycle recommendation**: Consolidation candidate (merge with dr-yyy.md into analysis document)

### stock/<project>/

#### <filename>.md
- ...

## Cross-file Analysis
- **Duplicates**: file1.md and file2.md overlap in area A (consolidation recommended)
- **Contradictions**: Figures in file3.md inconsistent with statements in file4.md
- **Gaps**: No file corresponds to area C
```

### research-plan.md

```markdown
# Research Plan
> Project: <project-name>
> Last updated: YYYY-MM-DD
> Current wave: N

## Coverage Assessment
| Area | Coverage | Supporting Files | Assessment |
|------|----------|-----------------|------------|
| A. xxx | 80% | file1.md, file2.md | Integrated analysis done |
| B. xxx | 40% | dr-xxx.md | DR output only |
| C. xxx | 0%  | — | Not started |

## Wave N (YYYY-MM-DD)
### New Research Items
| # | Research Item | Method | Priority | Status | Output |
|---|-------------|--------|----------|--------|--------|
| QN-1 | ... | DR | High | pending | dr-xxx.md |
| QN-2 | ... | Analysis | Medium | pending | xxx.md |

### DR Prompts
#### QN-1: [Research item name]
- Objective: ...
- Prompt (English):
  > ...
- Expected output: ...
- Save to: flow/<project>/dr-<topic>.md
```

### Incremental Update Mode (when existing files present)

**literature-review.md:**
- Update reviews for changed/added files, mark deleted files as "(deleted)"
- Re-evaluate cross-file analysis
- Recalculate review summary

**research-plan.md:**
- Update existing Wave statuses: if output file exists, change pending -> done
- Recalculate coverage assessment
- Add new Wave section (increment Wave number)
- Preserve past Wave sections (maintain as history)

## Phase 6: Finalize

1. Update README.md section 9 (Current Focus / Next Actions) with latest plan summary
   - Reflect high-priority research items
2. `git add flow/<project>/literature-review.md flow/<project>/research-plan.md flow/<project>/README.md`
3. Results report:
   - Literature Review summary (file count, quality distribution, lifecycle recommendations)
   - Coverage change summary (comparison with previous in incremental mode)
   - New research item count and priority breakdown
   - If lifecycle recommendations exist, suggest using `/promote` `/archive`
   - "Run `/expand <project>` again after completing research to re-evaluate"
   - "Changes are not committed. Run `git commit` after review."
