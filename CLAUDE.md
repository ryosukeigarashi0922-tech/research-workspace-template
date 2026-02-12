# Research Workspace — Claude Code Context

## Organization
> Edit `stock/reference/context/org-knowledge.md` with your organization's details.
> The section below is a quick summary for Claude Code's context window.

- Name: [YOUR ORGANIZATION NAME]
- Mission: [1-2 sentence mission statement]
- Key members: [Key people and their roles]
- Key partners: [Partner organizations]

## Document Style Guide
- [Define your language preferences: e.g., bilingual Japanese-English]
- [Define document styles by type: e.g., board materials, proposals, declarations]

## Directory Structure: flow / stock / archive

Manage information by lifecycle stage, organized by project.

| Layer | Path | Contents | External sync |
|---|---|---|---|
| inbox | `inbox/` | Temporary holding for external files (flat structure) | - |
| flow | `flow/<project>/` | DR outputs (`dr-*.md`), interim analysis, drafts | - |
| stock | `stock/<project>/` | Reviewed final deliverables | Optional |
| stock | `stock/reference/` | Templates, guides, white papers | - |
| stock | `stock/work-reports/YYYY-MM/` | Monthly work reports | Optional |
| stock | `stock/grants/<name>/` | Grant applications | Optional |
| archive | `archive/` | Superseded or completed projects | - |

### File Placement Rules (follow when creating new files)
0. Unsorted external files -> `inbox/`, route with `/intake`
0.5. Research plan -> `flow/<project>/research-plan.md` (generated/updated by `/expand`)
0.6. Literature review -> `flow/<project>/literature-review.md` (generated/updated by `/expand`)
0.7. File curation -> Execute literature-review.md recommendations with `/curate`
1. DR raw output -> `flow/<project>/dr-<topic>.md`
2. Interim analysis/draft -> `flow/<project>/<name>.md`
3. Review complete -> `git mv` to `stock/<project>/`
4. Work reports -> `stock/work-reports/YYYY-MM/<person>.md`
5. Shared reference material -> `stock/reference/{templates,guides,context,white-papers}/`
6. Obsolete intermediates -> Move to `archive/` or delete

### Naming Conventions
- `kebab-case.md`, no numeric prefixes, DR outputs require `dr-` prefix

### README.md (PMBOK 7-aligned Working Brief)
- `flow/<project>/README.md`: Working brief (sections 1-7: Purpose through Success Criteria + sections 8-9: File inventory & Next actions)
- `stock/<project>/README.md`: MoC (Overview, deliverable list, recommended reading order, related resources)
- Generate with `/new-project`, convert to stock version with `/promote`
- Update README.md file inventory when adding/updating files

## Quality Standards (all deliverables)
- Coverage: Every input requirement has a corresponding description
- Internal consistency: No contradictions in facts/figures within a document
- Format compliance: Follows specified output format
- Language quality: Natural writing style, accurate use of domain terminology

## Context Management
Load files in this priority order:
1. Required: Essential for task execution (CLAUDE.md, call-for-proposals, review criteria, etc.)
2. Reference: Improves quality (past applications, success examples, etc.)
3. Supplementary: Load on demand (full annual reports, etc.)

For domain knowledge, load `stock/reference/context/org-knowledge.md`.
For large documents (100+ pages), create a summary version first.

### Domain Knowledge Updates
When promoting/updating files to stock/, check if the content contains new organizational facts (personnel, partners, funding, events) and propose updates to `stock/reference/context/org-knowledge.md`.

## Deep Research Integration
When Claude Code's built-in research capabilities (WebSearch, Grep, file reading, etc.) are insufficient, request Deep Research from the user.

### Criteria for Deep Research
- Latest trends, news, or statistics needed (beyond Claude Code's knowledge cutoff)
- Cross-referencing multiple primary sources required
- Detailed latest call-for-proposals or review criteria from specific foundations
- Comprehensive survey of academic papers or policy documents needed
- Fact-checking reliability is particularly important

### Request Format
When requesting Deep Research from the user, provide:
1. **Research objective**: Why this research is needed (1-2 sentences)
2. **Prompt (English)**: English prompt ready to paste into Deep Research
3. **Expected output**: What information would be sufficient
4. **Save location**: Save under `flow/<project>/` (e.g., `flow/<project>/dr-<topic>.md`)

### After Receiving Results
- User pastes into chat or saves to file and shares -> Claude Code integrates and processes
- For long results, suggest a save path and encourage file creation
- Cross-reference with existing CLAUDE.md context and report any contradictions or updates needed
