# gptcompany Organization Configuration

This repository contains organization-wide GitHub configurations and reusable workflows.

## Reusable Workflows

### AI Code Review (`_reusable-ai-review.yml`)

Automated code review using Claude Code on self-hosted runners.

**Usage:**
```yaml
name: AI Code Review
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  review:
    uses: gptcompany/.github/.github/workflows/_reusable-ai-review.yml@main
    with:
      review-focus: "security,patterns"
```

**Inputs:**
- `review-focus`: Comma-separated focus areas (default: `security,logic,patterns`)
- `max-files`: Maximum files to review (default: `20`)

**Requirements:**
- Self-hosted runner with `[self-hosted, shared]` labels
- Claude Code installed on runner with active Claude Max subscription

### Sync Planning (`_reusable-sync-planning.yml`)

Automatically sync GSD/Speckit planning artifacts to GitHub Issues and Milestones.

**Usage:**
```yaml
name: Sync Planning
on:
  push:
    paths:
      - '.planning/**'
      - 'tasks.md'

jobs:
  sync:
    uses: gptcompany/.github/.github/workflows/_reusable-sync-planning.yml@main
```

**With options:**
```yaml
jobs:
  sync:
    uses: gptcompany/.github/.github/workflows/_reusable-sync-planning.yml@main
    with:
      framework: 'auto'        # auto, gsd, or speckit
      create-project: true     # Create GitHub Project board
      sync-todos: false        # Also sync .planning/todos/
```

**Inputs:**
- `framework`: Which framework to sync (`auto` detects from files, `gsd`, or `speckit`)
- `create-project`: Create GitHub Project board if missing (default: `true`)
- `sync-todos`: Also sync todos from `.planning/todos/` (default: `false`)

**Auto-detection:**
- `.planning/ROADMAP.md` present → GSD framework
- `tasks.md` present → Speckit framework

**What gets synced:**
| Framework | Artifact | GitHub |
|-----------|----------|--------|
| GSD | Phases | Milestones |
| GSD | Plans | Issues |
| Speckit | Tasks | Issues |

## Repository Structure

```
.github/
└── workflows/
    ├── _reusable-ai-review.yml      # AI-powered PR review
    └── _reusable-sync-planning.yml  # Planning → GitHub sync
```
