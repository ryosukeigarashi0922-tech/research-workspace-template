# Research Workspace Template

[Claude Code](https://claude.ai/claude-code) で研究・リサーチ管理ワークスペースを構築するためのテンプレートです。

## Features

- **4-layer file lifecycle**: `inbox/` -> `flow/` -> `stock/` -> `archive/`
- **8 custom slash commands**: プロジェクト初期化からファイル棚卸しまでの一連のワークフロー
- **PMBOK 7-aligned project management**: ワーキングブリーフとMoC（Map of Contents）による構造化
- **Deep Research integration**: 外部調査ツールとの連携プロトコル
- **Notion integration** (optional): プロジェクト情報の取得・同期

## Quick Start

### 1. Use this template

GitHub の "Use this template" ボタンでリポジトリを作成するか、手動でクローン:

```bash
git clone https://github.com/YOUR_USERNAME/research-workspace-template.git my-workspace
cd my-workspace
rm -rf .git && git init
```

### 2. Customize for your organization

1. **`CLAUDE.md`** の `## Organization` セクションを自組織の情報に書き換え
2. **`stock/reference/context/org-knowledge.md`** に組織のドメイン知識を記述
3. **`.claude/settings.json`** で必要なプラグインを有効化

### 3. Start working

```bash
claude  # Claude Code を起動

# プロジェクトを開始
/new-project my-research

# プロジェクトの状態を確認
/project my-research

# Literature Review + ギャップ分析
/expand my-research

# ファイル整理の実行
/curate my-research

# 成果物をstock/へ昇格
/promote my-research
```

## Directory Structure

```
.
├── CLAUDE.md                          # Workspace rules & context (CUSTOMIZE THIS)
├── .claude/
│   ├── commands/                      # Custom slash commands
│   │   ├── new-project.md             # /new-project — Project initialization
│   │   ├── project.md                 # /project — Project launcher & status
│   │   ├── expand.md                  # /expand — Literature Review + gap analysis
│   │   ├── curate.md                  # /curate — Batch file lifecycle actions
│   │   ├── promote.md                 # /promote — Graduate flow/ to stock/
│   │   ├── archive.md                 # /archive — Move to archive/
│   │   ├── intake.md                  # /intake — Route inbox/ files
│   │   └── work-report.md            # /work-report — Structured work reporting
│   └── settings.json                  # Plugin & permission settings
├── inbox/                             # Temporary holding for external files
├── flow/                              # Work-in-progress (per project)
├── stock/
│   ├── reference/
│   │   ├── templates/                 # Document templates
│   │   ├── context/                   # Organizational knowledge base
│   │   ├── guides/                    # Operational playbooks
│   │   └── white-papers/              # Strategic reference documents
│   ├── work-reports/                  # Monthly work reports (YYYY-MM/)
│   └── grants/                        # Grant applications
└── archive/                           # Completed/superseded projects
```

## Commands Reference

| Command | Description | Input | Output |
|---------|-------------|-------|--------|
| `/new-project` | Initialize a new project directory | Project name | `flow/<project>/README.md` |
| `/project` | Load project context & show status | Project name | Dashboard + suggested actions |
| `/expand` | Literature Review + gap analysis | Project name | `literature-review.md` + `research-plan.md` |
| `/curate` | Execute lifecycle recommendations | Project name | File promotions, archives, consolidations |
| `/promote` | Graduate deliverables to stock/ | Project name or file | `git mv` + stock README update |
| `/archive` | Archive completed projects | Project name or file | `git mv` to `archive/` |
| `/intake` | Route inbox files to proper locations | File name (optional) | `git mv` with rename proposals |
| `/work-report` | Generate structured work report | Staff name | `stock/work-reports/YYYY-MM/<person>.md` |

## Customization Guide

### CLAUDE.md

`CLAUDE.md` is the main configuration file. Customize these sections:

- **Organization**: Your org's name, mission, key members
- **Document Style Guide**: Language preferences, document type conventions
- **Directory Structure**: Modify layers if needed (e.g., remove `grants/` if not applicable)

### Commands

Commands in `.claude/commands/` are fully customizable:

- **`work-report.md`**: Most organization-specific. Edit the 10 business categories and interview framework to match your team structure
- **`new-project.md`**: Modify Notion DB field names if your Notion structure differs
- **Other commands**: Generally reusable as-is

### Domain Knowledge

`stock/reference/context/org-knowledge.md` is your organization's knowledge base. Include:

- Founding history and governance structure
- Core concepts, methodologies, and intellectual frameworks
- Key partnerships and collaborations
- Funding landscape and financial structure
- Strategic priorities and ongoing initiatives

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI
- Git
- (Optional) Notion MCP plugin for project data integration
- (Optional) Deep Research MCP plugin for comprehensive web research

## License

MIT
