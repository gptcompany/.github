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

## Repository Structure

```
.github/
└── workflows/
    └── _reusable-ai-review.yml  # AI-powered PR review
```
