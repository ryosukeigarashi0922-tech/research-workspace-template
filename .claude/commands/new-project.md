---
description: "Initialize a new project directory in flow/ with Notion data + interview-based README"
argument-hint: "[project-name]"
---

# New Project: PMBOK 7-aligned Working Brief + Project Initialization

Create a new project working directory under flow/ and generate a README.md (working brief) through an interview process.

Project name: $ARGUMENTS (ask if not specified)

Template reference: `stock/reference/templates/project-charter-flow.md`

## Phase 1: Project Name Confirmation

1. Confirm project name (kebab-case) from $ARGUMENTS or through dialogue
2. Check for duplicates in flow/ and stock/

## Phase 2: Notion Pre-research

1. Search Notion Projects DB (match by name, Japanese name, etc.)
   - Use Notion MCP search tools
2. If found:
   - Retrieve metadata: Name, Title, Status, Type, Strategic Rationale, Lead, Partners, Timeframe
   - Retrieve page body: Part I (sections 1-7: Overview, Objectives, Scope, Stakeholders, Risks, Deliverables, Success Criteria)
   - Present retrieved data to user and confirm accuracy
   - Use approved data as "known information" in Phase 3
3. If not found:
   - Report "No matching project found in Notion"
   - Proceed to full interview in Phase 3

## Phase 3: Interview

For information already retrieved from Notion, use confirmation-only format: "We understand X — any additions or corrections?"
For local-specific information (what to research, what files to create), always ask directly.

Question areas (PMBOK 7 Performance Domain mapping):
a. **Purpose** — What is being investigated in this local workspace [Project Work]
   (Only differences from Notion section 1)
b. **Objectives** — Specific goals to achieve [Planning]
   (Only differences from Notion section 2)
c. **Scope** — Range covered and excluded locally [Planning]
   (Confirm local-specific boundaries based on Notion section 3)
d. **Stakeholders** — Deliverable readers, information providers [Stakeholders / Team]
   (Extract from Notion section 4 + local-specific)
e. **Risks & Constraints** — Research constraints and risks [Uncertainty]
   (Only locally relevant items from Notion section 6)
f. **Deliverables** — What this workspace will produce [Delivery]
   (Local-specific: always ask fully)
g. **Success Criteria** — When is the research/analysis complete [Measurement]
   (Local-specific: always ask fully)

### Operating Rules
- Notion-covered areas: confirmation only (within 1 minute)
- Local-specific areas (f, g): ask thoroughly (within 3 minutes each)
- Undecided items: mark as [TBD]
- Target total: 10-15 minutes

## Phase 4: README.md Generation + Directory Creation

1. `mkdir -p flow/<project>/`
2. Generate flow README.md from interview results
   - Follow template `stock/reference/templates/project-charter-flow.md`
   - Section 8 (File inventory): empty table
   - Section 9 (Current focus): infer from interview
3. `git add flow/<project>/README.md`
4. Guide:
   - "Save DR outputs as `dr-<topic>.md`, analyses as `<name>.md`"
   - "After deliverables are finalized, use `/promote <project>` to graduate to stock/"
