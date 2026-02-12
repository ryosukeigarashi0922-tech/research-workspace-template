---
description: "Read inbox/ files and semi-automatically route them to appropriate flow/stock/ locations"
---

# Intake: inbox -> flow / stock Semi-automatic Routing

Read external files temporarily stored in inbox/, propose appropriate destinations, and move after user approval.

Target: $ARGUMENTS (filename or glob. If not specified, process all files in inbox/)

## Step 1: Scan inbox/

1. List files in `inbox/` (exclude `.gitkeep`)
2. If 0 files: report "No files in inbox/" and exit
3. If $ARGUMENTS specified, target only matching files

## Step 2: Read and Classify File Contents

For each file:

1. Read the beginning (~100 lines) and determine:
   - **Content summary**: 1-2 sentences describing what's written
   - **File type**: DR output / analysis-draft / final deliverable / reference material / work report / other
   - **Related project**: Correspondence with existing flow/ or stock/ projects
   - **Language**: Primary language of the document

2. Propose destination:
   - DR output -> `flow/<project>/dr-<topic>.md`
   - Interim analysis -> `flow/<project>/<name>.md`
   - Final deliverable -> `stock/<project>/<name>.md` (confirm if OK to place directly in stock)
   - Reference material -> `stock/reference/{context,guides,templates,white-papers}/<name>.md`
   - Work report -> `stock/work-reports/YYYY-MM/<person>.md`
   - Cannot determine -> Ask user directly

3. Rename proposal:
   - Propose conversion to kebab-case if needed
   - Propose `dr-` prefix for DR outputs
   - Follow existing naming conventions

## Step 3: Present Proposals

Present all proposals in table format:

```
| # | Original File | Type | Proposed Destination | Summary |
|---|---------------|------|---------------------|---------|
| 1 | report.md | Analysis | flow/<project>/market-analysis.md | Market analysis |
| 2 | ... | ... | ... | ... |
```

Confirm with user:
- Approve / modify / skip each proposal
- If a new project directory is needed, suggest using `/new-project`
- If destination project directory doesn't exist yet, propose creation

## Step 4: Execute

For approved proposals:

1. Create destination directory if needed with `mkdir -p`
2. If rename needed: `git mv inbox/<old-name> <destination>/<new-name>`
3. If no rename needed: `git mv inbox/<file> <destination>/`
4. Stage changes with `git add`

## Step 5: Update README.md

If destination has a README.md:
1. Add the new file to the file inventory table (section 8)
2. `git add <README.md>`

If no README.md exists:
- Suggest "No README.md found. Create one with `/new-project <project>`"

## Step 6: Report Results

- Display results with `git status`
- List any remaining files in inbox/
- Note "Changes are not committed. Run `git commit` after review."
