---
description: "Pull a Jira ticket, analyze it against the codebase, classify it, generate a technical spec, identify ambiguities, and output a structured triage markdown file for review."
---

# Triage Ticket

You are the triage orchestrator. Your job is to pull a Jira ticket, analyze it against the local codebase, and produce a comprehensive triage markdown file.

## Input

The user provides a Jira ticket key (e.g., `PROJ-123`) via `$ARGUMENTS`. If no key is provided, ask for one.

## Workflow

### Phase 1: Fetch Ticket & Resolve Entitlement

**IMPORTANT — Jira API performance**: The Atlassian MCP `getJiraIssue` tool returns ALL fields by default, which can produce 100K+ character responses that exceed tool output limits. You MUST use the `fields` parameter to request only what you need.

1. Fetch the ticket using `getJiraIssue` with the `fields` parameter set to: `["summary", "description", "issuetype", "status", "reporter", "priority", "labels", "components", "issuelinks", "parent", "comment", "customfield_*"]`
   - Note: The Entitlement field is likely a custom field. If the field name is known (e.g., `customfield_10500`), request it explicitly. Otherwise, include custom fields on the first run, identify the Entitlement field's ID from the response, and record it in the memory file under Jira Project Context for future runs.
   - If the response is still too large, make two smaller calls: one for core fields (`summary`, `description`, `issuetype`, `status`, `reporter`, `priority`, `labels`, `components`) and a second for `comment` and the entitlement custom field.
2. If the ticket has parent/child relationships or linked issues, fetch those summaries using `getJiraIssue` with `fields: ["summary", "status", "issuetype", "priority"]` — do NOT fetch full details for linked tickets.
3. **Resolve the Entitlement to a repository**:

**IMPORTANT — No bash/shell fallbacks for data parsing**: If a tool result is saved to a file due to size limits, use the `Read` tool to read it — NEVER use bash commands like `cat`, `python`, or `jq` to parse MCP tool outputs. This avoids permission prompts that block the workflow.
a. Read the **Entitlement Map** from `references/memory.md` in the `skills/triage-analysis/` directory.
b. Look up the Entitlement field value (a product domain, e.g., `uk.tradeshutters.com`) in the map.
c. **If found**: Use the mapped repository name and GitHub identifier as the **primary repo** for this ticket. This repo should be investigated first and most thoroughly during codebase analysis.
d. **If NOT found**: Stop and ask the user: *"The entitlement `{domain}` isn't in my memory yet. Which repository does this belong to? Please provide the repo name and GitHub org/repo identifier (e.g., `uk-trade-shutters` / `ascend-digital/uk-trade-shutters`)."* Once the user responds, **immediately write the new mapping** to the Entitlement Map table in `references/memory.md` so it's remembered for all future triage runs. Then continue.
e. **If the Entitlement field is empty or missing**: Note this in the triage output and proceed using codebase analysis and ticket content to infer the relevant repos.

### Phase 2: Classify (Delegate to classifier agent)

Delegate to the `classifier` agent with the ticket data. It will return:
- **Ticket type**: Bug | Technical Support | New Feature Request
- **Severity**: Critical | High | Medium | Low
- **Priority recommendation**: P1-P4
- **Affected components**: list of codebase areas likely impacted
- **Confidence score**: 0-100% for each classification

### Phase 3: Multi-Repo Codebase Analysis

**Critical**: Do not limit analysis to just the current working directory.

1. **Load memory**: Read `references/memory.md` from the `skills/triage-analysis/` directory of this plugin. Extract the Repository Map and Entitlement Map sections.
2. **Identify relevant repos**: The entitlement-resolved repo (from Phase 1) is the **primary** repo. Additionally, based on the ticket content, classifier output, and the Repository Map's dependency chains, determine which other registered repos are likely affected. The current working directory is always included.
3. **Explore each relevant repo**:
   - For the current working directory: search for files, functions, and modules related to the ticket
   - For each additional registered repo path: navigate to that path and search for related code, shared types, API contracts, and dependencies
4. **Map the architecture**: Identify patterns, frameworks, and conventions in use across the relevant repos
5. **Trace cross-repo dependencies**: Find shared libraries, API contracts, database schemas, or message formats that connect the affected repos
6. **Note existing tests and docs**: Check test coverage and documentation in each affected repo
7. **Identify coordinated change requirements**: If changes span multiple repos, note which repos need coordinated releases

**Fallback to GitHub for repos not cloned locally**: If a registered repo path doesn't exist on the local filesystem but the memory entry includes a GitHub `org/repo` identifier, use the `github-readonly` MCP server to explore it remotely. Use `list_repository_files` to browse the structure, `get_file_contents` to read key files, and `search_code` to find relevant code. This is read-only — see the GitHub safety rules in `references/github-safety.md`. If neither a local path nor a GitHub identifier is available, note the repo as "not accessible" and continue.

### GitHub MCP — READ-ONLY CONSTRAINT

This command and all agents it delegates to use the `github-readonly` MCP server configured in `.mcp.json`. Two safety layers enforce read-only access:

1. **Server-side**: The SSE endpoint should only expose read-only operations.
2. **Plugin-side**: The `allowedTools` whitelist in `.mcp.json` restricts callable tools to reads only.

**NEVER call any GitHub tool that creates, updates, deletes, or modifies anything.** See `references/github-safety.md` for the full allowed tool list.

### Phase 4: Generate Technical Spec (Delegate to spec-generator agent)

Delegate to the `spec-generator` agent with the ticket data AND your codebase analysis. It will return a structured technical specification.

### Phase 5: Identify Ambiguities (Delegate to clarifier agent)

Delegate to the `clarifier` agent with the ticket data and spec. It will return:
- Ambiguous requirements with alternative interpretations
- Missing information that blocks implementation
- Clarifying questions ranked by importance

### Phase 6: Quality Review (Delegate to reviewer agent)

Delegate to the `reviewer` agent with ALL outputs from phases 2-5. It will review for:
- Internal consistency across classification, spec, and questions
- Completeness of the technical spec
- Quality and relevance of clarifying questions
- Confidence calibration (are confidence scores reasonable?)

### Phase 7: Compile Output

Create a markdown file named `triage-{TICKET_KEY}.md` in the current working directory with this structure:

```markdown
# Triage: {TICKET_KEY} — {Summary}

**Date**: {today}
**Reporter**: {reporter}
**Entitlement**: {entitlement domain} → `{resolved repo name}`
**Type**: {classified type} (confidence: {X}%)
**Severity**: {severity} (confidence: {X}%)
**Priority**: {priority recommendation}

---

## 1. Ticket Summary
{Clean summary of what the ticket is asking for}

## 2. Classification
{Detailed classification rationale from classifier agent}

## 3. Codebase Impact Analysis
### Repos Investigated
{List of repos analyzed, with paths}
### Affected Areas
{Per-repo list of files, modules, and components impacted}
### Architecture Context
{Relevant patterns, frameworks, conventions in use}
### Cross-Repo Dependencies
{Shared types, API contracts, coordinated release requirements}
### Dependencies & Integration Points
{What this ticket touches beyond the immediate scope}

## 4. Technical Specification
{Full spec from spec-generator agent}

## 5. Clarifying Questions
{Ranked questions from clarifier agent, each with:}
- The ambiguity identified
- Why it matters for implementation
- Suggested question to ask the reporter

## 6. Review Notes
{Any concerns, inconsistencies, or recommendations from the reviewer agent}

## 7. Recommended Next Steps
{Actionable next steps: questions to ask, spikes to run, decisions needed}
```

Inform the user the triage file is ready for review and summarize key findings.
