---
name: triage-analysis
description: "Codebase-aware Jira ticket triage analysis. Automatically activates when analyzing a Jira ticket against a local codebase to determine impact, affected components, and implementation complexity. Use when triaging tickets, assessing bug reports, evaluating feature requests, or scoping technical support issues against the current project."
---

# Triage Analysis Skill

## Codebase Exploration Strategy

When analyzing a ticket against the codebase, follow this exploration order:

1. **Understand the project**: Read README, package.json/pyproject.toml/Cargo.toml (or equivalent) to understand the tech stack and structure.
2. **Map the architecture**: Identify entry points, main modules, and the dependency graph. Check for `references/memory.md` in the plugin directory for pre-defined repo relationships and domain terms.
3. **Search for relevance**: Use grep/glob to find code related to the ticket's keywords, feature area, or error messages.
4. **Trace the call chain**: From relevant entry points, trace how data flows through the affected area.
5. **Check test coverage**: Look for existing tests in the affected area to understand current coverage and expected behavior.
6. **Identify ripple effects**: Check what imports/depends on the affected code to assess blast radius.

## Impact Assessment Matrix

| Factor | Weight | How to Assess |
|--------|--------|---------------|
| Files changed | High | Count of files likely needing modification |
| Test coverage | High | Are affected areas well-tested? |
| Dependency depth | Medium | How deep in the dependency chain? |
| API surface | High | Does this change public APIs? |
| Data model | High | Does this change data schemas? |
| Cross-repo | High | Does this affect other repos? (check memory) |

## Output Expectations

The triage analysis should produce:
- A map of affected files with confidence levels
- Architecture context (patterns in use, frameworks, conventions)
- Complexity estimate: Simple (<1 day) / Moderate (1-3 days) / Complex (3+ days)
- Risk factors specific to this codebase

## Memory Integration

Always check `references/memory.md` for:
- **Domain glossary**: Terms that have specific meanings in this organization
- **Repo map**: How repositories relate to each other (shared libraries, microservices, monorepo structure)
- **Team conventions**: Naming patterns, branching strategies, review requirements
- **Known debt**: Areas of technical debt that affect estimation
