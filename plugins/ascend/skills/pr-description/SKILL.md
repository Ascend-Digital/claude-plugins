---
name: pr-description
description: "Generates PR descriptions using the team's PR template. Automatically activates when creating a pull request, filling a PR template, or generating a PR description. Use when the user asks to create a PR, fill a PR template, prepare a merge request description, or summarize branch changes for review."
---

# PR Description Generator

Populate the team's PR template using git context, Jira ticket data, and any existing
triage/spec files.

## Workflow

### 1. Detect Branch & Target

Run `git branch --show-current` to get the current branch name. The target branch defaults
to `main` unless the user specifies otherwise.

Extract a Jira ticket key from the branch name by matching patterns like:
- `feature/CS-1234-description`
- `bugfix/CS-1234-description`
- `CS-1234-description`
- `CS-1234`

The pattern is: any prefix ending in `/` (optional), followed by a ticket key matching
`[A-Z]+-\d+`.

If no ticket key is found in the branch name, check commit messages for a ticket key pattern.
If still not found, proceed without Jira context.

### 2. Gather Git Context

Run these commands to understand what changed:

```bash
git log <target>..HEAD --oneline
git diff <target>...HEAD --stat
git diff <target>...HEAD
```

Use the commit messages to understand the narrative of changes. Use the diff to understand
the actual code modifications.

If `HEAD` is the same as the target branch (no commits ahead), inform the user:
"No changes detected against `<target>`. Are you on the right branch?"
and stop.

### 3. Gather Jira Context (if ticket key found)

Fetch the Jira ticket using the Atlassian MCP `getJiraIssue` tool with fields:
`["summary", "description", "issuetype", "status", "labels", "components"]`

Use the ticket summary and description to understand the motivation behind the changes.

### 4. Check for Triage/Spec Files

Look in the current working directory for:
- `triage-{TICKET_KEY}.md`
- `spec-{TICKET_KEY}.md`
- `TRIAGE_*_{TICKET_KEY}.md`

If found, read the "Understanding of the Request" section (from triage) or the summary
section (from spec) to enrich the motivation context.

### 5. Read the Template

Read `references/template.md` from this skill's directory to get the exact PR template.

### 6. Populate the Template

Fill in the template sections:

**Motivation**: Write a concise explanation of what influenced this change. Source priority:
1. Jira ticket summary + description (if available)
2. Triage file's "Understanding of the Request" (if available)
3. Summarise from commit messages as a fallback

**Changes**: Create a bullet list of the significant code changes. Group by logical change
area (e.g., "Added new OrderService for order processing", "Updated migration to add
status column"), not by individual file. Focus on what's significant — new paradigms,
architectural decisions, non-obvious choices.

**Notes**: Only populate if there is genuinely relevant context such as:
- Future work that will follow this PR
- FE/BE counterpart work that accompanies this
- Known trade-offs or temporary solutions
If nothing applies, leave the section empty (keep the heading and HTML comment).

**Testing**: Leave as the placeholder `- ` for the user to fill manually.

**Reminders (Author)**: Always include the full checklist exactly as it appears in the
template, with all checkboxes unchecked.

### 7. Present to User

Output the completed PR description as a single markdown block. Do NOT automatically create
the PR — present it for the user to review, edit, and use as they see fit.

If the user then asks to create the PR, they can copy it or you can help pass it to
`gh pr create` — but only on explicit request.
