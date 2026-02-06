---
description: "Analyze a Jira ticket for ambiguities and generate targeted clarifying questions to ask the reporter."
---

# Generate Clarifying Questions

Analyze a Jira ticket for ambiguous, incomplete, or contradictory requirements and produce targeted clarifying questions.

## Input

`$ARGUMENTS` should be a Jira ticket key (e.g., `PROJ-123`).

## Workflow

1. **Fetch ticket** via Atlassian MCP tools. Pull description, comments, and acceptance criteria.

2. **Check for existing triage**: Look for `triage-{TICKET_KEY}.md` in the working directory to incorporate prior analysis.

3. **Analyze codebase**: Briefly explore related code to understand what information is needed for implementation.

4. **Delegate to clarifier agent** with ticket data and codebase context.

5. **Output**: Write clarifying questions to `triage-{TICKET_KEY}.md` (updating the Clarifying Questions section) or to `clarify-{TICKET_KEY}.md` if no triage file exists.

Each question should include:
- The specific ambiguity or gap identified
- Why this information is needed (impact on implementation)
- The question to ask, written clearly for a non-technical reporter
- Suggested answer options where applicable (to reduce back-and-forth)
