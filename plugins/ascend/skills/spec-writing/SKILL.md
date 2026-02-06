---
name: spec-writing
description: "Technical specification writing for Jira tickets. Automatically activates when generating implementation specs, architecture recommendations, or acceptance criteria from requirements. Use when creating technical specs, writing implementation plans, defining acceptance criteria, or documenting architecture decisions for tickets."
---

# Spec Writing Skill

## Spec Quality Principles

1. **Traceable**: Every requirement traces to something in the ticket. Don't invent scope.
2. **Testable**: Every acceptance criterion can be verified with a concrete test.
3. **Implementable**: Another developer can build from this spec without guessing.
4. **Bounded**: Out-of-scope is explicit. Scope creep is the enemy.
5. **Context-aware**: References actual file paths, function names, and patterns from the codebase.

## Writing Guidelines

### For Bugs
- Include reproduction steps (from ticket or inferred from code)
- Document expected vs actual behavior
- Identify the likely root cause area in the codebase
- Spec should focus on the fix approach, not just describing the bug

### For New Feature Requests
- Start with user stories: "As a [role], I want [action], so that [benefit]"
- Break into functional requirements (what it does) and non-functional (how well it does it)
- Architecture recommendation should extend existing patterns, not introduce new ones without justification
- Include data model changes with migration strategy

### For Technical Support
- Spec becomes a diagnostic and resolution plan
- Include troubleshooting decision tree
- Document root cause analysis approach
- Spec the fix OR the workaround, whichever is appropriate

## Cross-Repo Awareness

When the memory file indicates cross-repo dependencies:
- Note which repos are affected in the Integration Points section
- Flag if changes require coordinated releases
- Identify shared libraries or contracts that need updates
- Reference the memory file's repo map for accurate dependency chains

## Reference Files

For detailed specification templates and examples, see:
- `references/spec-template.md` — Full template with field descriptions
