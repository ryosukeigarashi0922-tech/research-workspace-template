---
description: "Graduate deliverables from flow/ to stock/ (git mv + git add + stock README generation)"
argument-hint: "[project-name]"
---

# Promote: flow -> stock

Graduate deliverables from flow/ to stock/.

Target: $ARGUMENTS (project name or file path. Ask if not specified)

## Steps

### Step 1: Identify Target
- If project name specified -> Show file list in `flow/<project>/`
- If file path specified -> Target that file
- If not specified -> List all projects under `flow/` and ask user to select

### Step 2: Select Files for Promotion
- Display all files in the project and confirm which to promote
- Note that `dr-` prefixed files (DR raw output) are typically not promotion candidates
- Only promote user-selected files

### Step 3: Execute
- Create `stock/<project>/` directory if it doesn't exist
- Run `git mv flow/<project>/<file> stock/<project>/<file>` for each selected file
- Stage changes with `git add`

### Step 4: Generate/Update stock README.md

Template reference: `stock/reference/templates/project-charter-stock.md`

**If README.md doesn't exist:**
1. If `flow/<project>/README.md` exists, convert section 1 (Purpose) and section 6 (Deliverables) to MoC format
2. If no flow README exists, read the beginning of promoted files to generate an overview and confirm with user
3. Populate file inventory table with promoted files
4. Confirm recommended reading order with user
5. `git add stock/<project>/README.md`

**If README.md already exists:**
1. Add newly promoted files to the file inventory table
2. Check if recommended reading order needs updating
3. `git add stock/<project>/README.md`

### Step 5: Domain Knowledge Update Check
- Check if promoted files contain new organizational facts (personnel, partners, funding, events, etc.)
- If so, propose specific updates to `stock/reference/context/org-knowledge.md` and get user approval before applying

### Step 6: Verify
- Display results with `git status`
- If flow/<project>/ is now empty, suggest deleting the directory
