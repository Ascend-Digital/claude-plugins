---
description: "Generate a PR description using the team's template, populated from git diff, commit history, Jira ticket, and any existing triage/spec files."
---

# Generate PR Description

## Input

`$ARGUMENTS` is optional. It can be:
- A target branch name (e.g., `develop`) — defaults to `main` if not provided
- A Jira ticket key (e.g., `CS-1234`) to override auto-detection from branch name
- Both, space-separated (e.g., `develop CS-1234`)

## Workflow

1. Parse `$ARGUMENTS`:
   - If a value looks like a branch name (no `-` between uppercase letters and digits), treat
     it as the target branch
   - If a value matches `[A-Z]+-\d+`, treat it as a ticket key override
   - If empty, use defaults (target: `main`, ticket key: auto-detect from branch name)

2. Invoke the `pr-description` skill with the resolved target branch and optional ticket key
   override.
