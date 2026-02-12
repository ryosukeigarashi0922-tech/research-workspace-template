---
description: "Generate structured work reports via Notion data retrieval + adaptive interview (PMBOK 7-aligned)"
---

# Work Report Interview

Extract work information from team members through an interview format and generate structured individual reports. Minimize re-confirmation of known information through Notion MCP pre-retrieval.

Target staff: $ARGUMENTS (confirm first if not specified)

---

## Purpose and Audience

| Purpose | Audience | Required Information |
|---------|----------|---------------------|
| **Performance reporting** | Management | Specific achievements, strategic contributions, above-and-beyond work |
| **Workload visibility** | Management | Structural issues, single points of failure, workload concentration |
| **Budget/staffing justification** | Management | Quantitative evidence, opportunity costs, risk scenarios |

## Framework

PMBOK 7 — **Team Performance Domain** + **Portfolio Resource Management**
- **Capacity**: What each person actually does and how much time it requires
- **Demand**: Current workload, pipeline, future projections
- **Gap**: Unrealized value, structural bottlenecks, risks

---

## Execution Flow

### Phase 1: Pre-data Collection and Approval

Use Notion MCP to retrieve:

1. **Existing reports**: Search by staff name -> past work reports
2. **Project involvement**: Projects DB -> projects the person is involved in
3. **External activities**: Engagement/activity records
4. **Meeting notes**: Related meeting minutes
5. **Task status**: Active tasks

**Present all retrieved data to user for approval.**
Notion data may be outdated, inaccurate, or incomplete — do not automatically treat as "known information."

**After user approval:**
- Use only approved data as "known information"
- Apply corrections/supplements
- Ignore rejected data
- Determine coverage status for each interview area:
  - **Sufficient**: Approved data is enough -> confirm only during interview
  - **Partial**: Some data available but gaps exist -> focus questions on gaps
  - **Insufficient**: No data -> full questioning

### Phase 2: Interview

**Opening:**

> I'll ask about your work situation. The purpose is to accurately reflect your achievements in evaluation and communicate team issues to management.
>
> I'll handle the structuring, so please feel free to answer as things come to mind. If you don't have exact numbers, rough estimates are fine.
>
> I'll focus on areas not already covered by Notion data, keeping confirmation of known information brief.

Confirm name/title (confirm only if known) and agree on target period (default: past 6 months).

---

**Collect information across the following areas. Order is not fixed.**

Follow the respondent's natural flow, distributing cross-area responses appropriately. For Notion-covered areas, use confirmation format only: "We understand X — any additions or corrections?"

**Allocate ~3 minutes per area. Do not spend more than 7 minutes on any single area. Notion-covered areas: within 1 minute.**

#### Area A: Key Achievements and Strategic Contributions

**Information to collect:**
- Biggest achievements in this period (quantified: funding secured, MOUs signed, events organized, proposals written, etc.)
- Contributions to core strategic initiatives
- Work performed beyond official job scope and its outcomes

**Completion criteria:**
- [ ] 3+ specific achievements (at least 1 quantified)
- [ ] Core initiative contributions identified
- [ ] Related project names identified

#### Area B: Work Portfolio

**Category-based breakdown:**

> CUSTOMIZE: Replace the categories below with your organization's business categories.

| # | Category | Description |
|---|----------|-------------|
| 1 | Strategic Planning & Governance | Overall strategy, roadmap, governance |
| 2 | External Partnerships | Partner relations, negotiations |
| 3 | Funding & Proposals | Grant/proposal writing and coordination |
| 4 | Program Management | Program design, project progress management |
| 5 | Administration (HR, Budget) | HR, budget, asset management |
| 6 | Research & Publications | Own research, papers, publications |
| 7 | Cross-functional & Ad-hoc | Requests from other teams, out-of-scope work |

**Information to collect:** Specific tasks per category, time allocation %, routine vs. ad-hoc

**Completion criteria:**
- [ ] All categories marked "active" or "not active"
- [ ] Time allocation totals roughly 100% (exceeding = evidence of overload)
- [ ] Each category has specific content

#### Area C: Current Workload and Transferability

**Information to collect:**
- Current items (by load: item name, related project, load level, deadline, substitutability)
- Transferability classification:
  - **A: Only this person** — Difficult to replace due to tacit knowledge, relationships, expertise
  - **B: Transferable** — Can be handled by others with adequate handover
  - **C: Delegatable** — Can be immediately transferred

**Completion criteria:**
- [ ] 5+ item list (with load level and deadlines)
- [ ] A/B/C classification complete
- [ ] Reasons stated for A-classified items

#### Area D: Capacity Constraints and Structural Issues

**Information to collect:**
- Bottleneck/inefficiency causes and specific business impact
- Out-of-scope work interruptions (examples and who should handle them)
- Single points of failure (tasks that completely stop during absence)
- Current utilization rate (self-reported, 100% = normal)

**Completion criteria:**
- [ ] Specific business impact for each bottleneck
- [ ] Single points of failure identified
- [ ] Utilization rate stated

**Note:** This area often touches on overwork. Show empathy before proceeding.

#### Area E: Unrealized Value and Gaps

**Information to collect:**
- Work that should be done but isn't (especially research, strategic planning)
- Opportunity cost (specifically what is being lost)
- Resources needed to resolve (staffing, outsourcing, tools, budget)
- Staffing scenario: If +1 staff, what would be delegated first? How many hours/week freed, allocated to what?

**Completion criteria:**
- [ ] "Cannot do" items are specific
- [ ] Opportunity costs stated
- [ ] Delegation targets and freed time estimates for +1 scenario

#### Area F: Future Demand Forecast

**Information to collect:**
- Workload changes if pending proposals/initiatives are approved
- Certain workload changes in next 6-12 months
- If current staffing continues, what breaks first?

**Completion criteria:**
- [ ] 2+ pipeline projects and their impact
- [ ] 6-12 month outlook
- [ ] Breaking point (what, roughly when, what's affected first)

---

### Interview Operating Rules

**Follow-up questions:**
- Continue until sufficient information is gathered, but no more than 3 follow-ups on same topic
- Items respondent can't answer immediately: mark as `[TBD]` and move on
- Vague expressions like "several" or "various" — request quantification. If unavailable, use `~X (estimated)`

**Ambiguous number handling:**
- `~3 (self-estimated)` — Respondent's rough estimate
- `~5 (from Notion DB)` — Derived from existing data
- "Several" and "a few" are not acceptable in the final report

**Tone:**
- Dialogue, not interrogation. Show empathy for respondent's situation
- Use the respondent's preferred language

---

### Phase 3: Report Generation

After interview completion, create report with the following structure.

**Drafting rules:**
- Notation-based statements cite source: `(Projects DB)`, `(Engagement Reports)`, etc.
- Interview-sourced information needs no citation (default source)
- Insufficient sections get `[DATA GAP: specific missing content]`
- Ambiguous numbers: estimated value `~X` + source

```markdown
# Work Report: [Name] (YYYY-MM)

> Generated: YYYY-MM-DD | Source: Notion DB + Interview
> DATA GAPs remaining: X items

## 1. Profile & Role Summary
- Name, title, tenure
- Official job scope vs. actual scope

## 2. Key Achievements & Strategic Impact

### By Strategic Initiative
- [Initiative A] contributions
- [Initiative B] contributions

### Quantified Outputs
- Proposals: X submitted, $Y total
- Partnerships: X negotiated
- Events: X organized
- Research outputs: X papers/reports

## 3. Work Portfolio

### Category Breakdown
| # | Category | Time% | Nature | Key Activities | Projects |
|---|----------|-------|--------|----------------|----------|

### Time Allocation Summary
- Total: X%
- Routine vs. ad-hoc ratio

## 4. Current Workload

### Active Items (by priority)
| Item | Project | Load | Deadline | Substitutable? |
|------|---------|------|----------|----------------|

### Transferability Matrix
| Business Area | Class | Reason |
|---------------|-------|--------|
A = only this person, B = transferable, C = delegatable

## 5. Capacity Constraints & Structural Issues

### Bottlenecks
- [issue] -> [business impact]

### Out-of-Scope Work
- [task] -> [who should do it]

### Single Points of Failure
- [tasks only this person can do]

### Utilization Rate
- Self-reported: X%

## 6. Unrealized Value

### Should-Do-But-Can't
- [activity] -> [opportunity cost]

### Staffing Scenario (+1 staff)
- First task to delegate
- Estimated hours/week freed
- Planned reallocation

### Required Resources
- [resource type] -> [what it unlocks]

## 7. Future Demand Forecast

### Pipeline Impact
| Project | If adopted | Est. hours/week |
|---------|------------|-----------------|

### 6-12 Month Outlook
- [certain changes]

### Risk Scenario
- [what breaks first, when]

---
## Appendix: DATA GAPs
| # | Section | Gap Description | Action Required |
|---|---------|-----------------|-----------------|
```

### Phase 4: Quality Check

Self-verify before presenting report. Do not finalize until all criteria met:

**For performance evaluation:**
- [ ] 3+ specific achievements quantified
- [ ] Core initiative contributions stated
- [ ] Above-and-beyond contributions documented
- [ ] All related projects identified

**For workload visibility:**
- [ ] All portfolio categories covered (including "not active")
- [ ] Time allocation % stated (or DATA GAP)
- [ ] Single points of failure identified
- [ ] Utilization rate stated
- [ ] A/B/C transferability classification complete

**For budget justification:**
- [ ] Bottlenecks have specific business impact
- [ ] Opportunity costs stated
- [ ] Future demand increases tied to specific initiatives
- [ ] Breaking point scenario is specific
- [ ] +1 staffing scenario stated
- [ ] No ambiguous numbers remain

**General:**
- [ ] Project names are consistent with Projects DB
- [ ] DATA GAPs listed in Appendix

### Phase 5: Review and Save

1. Present report to user for confirmation
2. Address modification requests
3. After confirmation, save to `stock/work-reports/YYYY-MM/` (e.g., `stock/work-reports/2026-02/name.md`) -> git add
