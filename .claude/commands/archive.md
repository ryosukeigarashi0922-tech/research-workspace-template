---
description: "Move completed projects or obsolete files to archive/ (git mv + git add)"
argument-hint: "[project-name or file-path]"
---

# Archive: Completed Project Storage

Move completed projects or obsolete files to archive/.

Target: $ARGUMENTS (project name or file path. Ask if not specified)

## Steps

1. Identify target:
   - If project name specified -> Search both flow/ and stock/ for the project
   - If not specified -> List projects in flow/ and stock/ and ask for selection

2. Confirm archive scope:
   - flow/<project>/ only (cleaning up intermediates)
   - stock/<project>/ only (archiving completed deliverables)
   - Both (complete project archive)
   Ask user to confirm

3. Execute:
   - Create `archive/<project>/` directory
   - Move selected files with `git mv`
   - Stage changes with `git add`

4. Verify:
   - Display results with `git status`
