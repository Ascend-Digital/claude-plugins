---
description: "Generate or regenerate a technical specification for a Jira ticket, using codebase analysis and any new information from clarifying questions."
---

# Generate Technical Spec

Generate a detailed technical specification for a Jira ticket, incorporating codebase context and any new clarifications.

## Input

`$ARGUMENTS` should be a Jira ticket key (e.g., `PROJ-123`). Optionally, the user may also provide additional context or answers to previously asked clarifying questions.

## Workflow

1. **Fetch ticket** via Atlassian MCP tools. Pull full description, comments (which may contain answers to clarifying questions), and linked issues.

2. **Check for existing triage**: Look for `triage-{TICKET_KEY}.md` in the working directory. If found, read it to avoid duplicate work and incorporate prior analysis.

3. **Analyze codebase**: Explore files, modules, and tests related to the ticket. Identify architecture patterns, existing implementations to extend, and constraints.

4. **Delegate to spec-generator agent** with all context. The agent will produce a structured technical spec.

5. **Delegate to reviewer agent** for quality check on the spec.

6. **Output**: Write or update the spec section in `triage-{TICKET_KEY}.md`. If no triage file exists, create `spec-{TICKET_KEY}.md` as a standalone file.
