---
description: "View or edit the triage memory — domain glossary, repo map, team conventions, and known technical debt."
---

# Manage Triage Memory

View or update the ascend plugin's long-term memory, which stores domain definitions, repository relationships, team conventions, and known technical debt.

## Input

`$ARGUMENTS` controls the action:

- **No arguments**: Display the current memory contents with a summary
- **`add glossary <term> <definition>`**: Add a term to the Domain Glossary
- **`add convention <convention>`**: Add a team convention
- **`add debt <area> <description>`**: Add a known technical debt entry
- **`add entitlement <domain> <repo> <github> [notes]`**: Map an entitlement domain to a repository
- **`add jira <context>`**: Add Jira project context
- **`edit`**: Open the full memory file for manual editing

## Examples

```
/ascend:memory
/ascend:memory add glossary "Widget" "Customer-facing dashboard component in frontend-app/src/components/Widget/"
/ascend:memory add convention "PRs require 2 approvals minimum"
/ascend:memory add debt "auth module" "Legacy session-based auth, migration to JWT planned Q3"
/ascend:memory add entitlement "uk.tradeshutters.com" "uk-trade-shutters" "ascend-digital/uk-trade-shutters" "UK Trade Shutters storefront"
/ascend:memory add jira "Project key: PROJ, Sprint cadence: 2-week sprints"
/ascend:memory edit
```

## Workflow

1. Parse the action from `$ARGUMENTS`.
2. Locate `references/memory.md` in the `skills/triage-analysis/` directory of the plugin.
3. For **view**: Read and display the memory with a count of entries per section.
4. For **add**: Append the new entry to the appropriate section, replacing the placeholder text if it's the first entry.
5. For **edit**: Read the file, present it to the user, and apply their changes.
6. Confirm the update to the user.

## Why Memory Matters

The memory file is read by ALL agents and skills during triage. Keeping it up to date means:
- The classifier understands your domain terms (not generic definitions)
- The spec-generator knows which repos to reference for cross-repo impact
- The clarifier doesn't ask questions that your conventions already answer
- Risk assessment accounts for known technical debt
- **Entitlement resolution is instant** — the triage command maps product domains to repos without asking you, as long as the mapping exists in memory. When a new entitlement shows up, triage asks once, saves it, and never asks again.
