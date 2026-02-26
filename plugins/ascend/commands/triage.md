---
description: "Analyze a Jira ticket against the codebase and produce a three-section triage report: understanding of the request, how the system currently works, and clarifying questions."
---

# Triage Ticket

## Input

The user provides a Jira ticket key (e.g., `CS-6442`) via `$ARGUMENTS`. If no key is
provided, ask for one.

## Workflow

### Step 1: Load Memory

Read `references/memory.md` from the `skills/triage-analysis/` directory. Extract:

- **Entitlement Map** — domain-to-repo mappings
- **Repository Map** — repo details, dependencies, GitHub identifiers
- **Service Desk Custom Fields** — known Jira field IDs per project

### Step 2: Fetch the Jira Ticket

Use the Atlassian MCP `getJiraIssue` with fields:
`["summary", "description", "issuetype", "status", "reporter", "priority", "labels",
"components", "issuelinks", "parent", "comment", "attachment", "customfield_10744",
"customfield_10932"]`

Plus any Service Desk form fields from memory for this project (e.g., for project CS:
`customfield_10777`, `customfield_10778`, `customfield_10780`, `customfield_10781`,
`customfield_10782`).

**Critical**: If the `description` is empty and no custom fields are recorded for this
project, make a second fetch **without** the `fields` parameter to discover all fields.
Scan for content-bearing `customfield_*` entries, filter out boilerplate (single `-`, empty
templates, placeholder text), and record discoveries in `references/memory.md`.

Fetch linked issues and parent tickets with minimal fields:
`["summary", "status", "issuetype", "priority"]`

### Step 3: Resolve Repositories

1. Match the entitlement domain (from `customfield_10932`, or extracted from the
   `product.name` inside `customfield_10744`) to the **Entitlement Map**
2. If not found: ask the user for the correct repository, then write the mapping to memory
3. From the **Repository Map**, identify all repos to explore — the primary repo plus any
   it depends on

### Step 4: Explore and Analyse

Build a list of the primary repositories (GitHub `owner/name`) and any repository mappings
from memory. Then delegate exploration to a sub-agent using the Task tool with the following
prompt structure:

```
Primary repositories to explore:
- {org/repo-1}
- {org/repo-2}

Repository mappings:
{relevant entries from the Repository Map}

Instructions:
1. Use the Atlassian MCP to read the full Jira ticket {TICKET_KEY} — get the summary,
   description, acceptance criteria, comments, and any other relevant fields.
2. Use the GitHub MCP to perform a deep exploration of ALL the primary repositories listed
   above:
   - Read the README and directory structure to understand each project's layout
   - Trace through the code comprehensively to understand architecture and abstractions
   - Identify modules, services, and files most relevant to what the ticket describes
   - Read key files to understand the current implementation in detail
   - If you discover references to other repositories in the mappings above, or shared
     libraries/upstream services, explore those too via the GitHub MCP
3. Produce a markdown report with THREE sections:
   a. "## Understanding of the Request" — summarise what you understand the ticket is asking
      for, including the goal, scope, and any constraints mentioned
   b. "## How the System Currently Works" — a business-friendly explanation of how the
      relevant parts work today, using diagrams (Unicode box-drawing) where helpful
   c. "## Clarifying Questions" — if applicable, a numbered list of questions for the
      ticket reporter about ambiguities, missing details, or edge cases
4. Return your output as JSON (and nothing else) in this exact format:

{
  "understanding_of_request": "## Understanding of the Request\n\n...",
  "how_system_works": "## How the System Currently Works\n\n...",
  "clarifying_questions": "## Clarifying Questions\n\n1. ..."
}
```

Include the full compiled ticket content in the prompt so the sub-agent has context.

For multi-repo exploration, spawn one sub-agent per repo in parallel using
`run_in_background: true`, then collect results.

### Step 5: Output

Parse the JSON response and write `triage-{TICKET_KEY}.md` in the current working directory:

```markdown
# Triage: {TICKET_KEY} — {Summary}

**Date**: {today}
**Reporter**: {reporter}
**Entitlement**: {entitlement domain} → `{resolved repo name}`
**Repos explored**: {list}

---

{understanding_of_request}

{how_system_works}

{clarifying_questions}
```

Inform the user the triage file is ready and provide a brief summary of key findings.

## GitHub MCP — READ-ONLY CONSTRAINT

All exploration uses the `github-readonly` MCP server. Only read-only tools are permitted.
See `references/github-safety.md`. Never call any tool that creates, updates, deletes, or
modifies anything on GitHub.
