---
name: ambiguity-detection
description: "Detects ambiguities, gaps, and contradictions in Jira ticket requirements. Automatically activates when analyzing requirements for completeness, generating clarifying questions, or identifying underspecified tickets. Use when a ticket needs clarification, when requirements seem vague, or when generating questions for ticket reporters."
---

# Ambiguity Detection Skill

## Detection Method: Multi-Interpretation Analysis

For each requirement or statement in a ticket:
1. Generate 2-3 plausible alternative interpretations
2. If more than one interpretation is reasonable, flag as ambiguous
3. Assess the implementation impact of each interpretation diverging

This is more reliable than trying to spot ambiguity directly — it forces concrete thinking about what "could this mean something else?" looks like in practice.

## Common Ambiguity Patterns in Tickets

### Vague quantifiers
"Support **many** users" → How many? 100? 10,000? 1M?
"Should be **fast**" → <100ms? <1s? <5s?
"**Most** cases should work" → 80%? 95%? 99.9%?

### Implicit assumptions
"Update the dashboard" → Which dashboard? For which role?
"Fix the API" → Which endpoint? What's broken?
"Like the old system" → Which version? Which behavior specifically?

### Missing error handling
"User submits the form" → What if validation fails? What if the server is down?
"Process the file" → What if the file is malformed? Too large? Wrong format?

### Scope boundaries
"Add search" → Full-text? Filters only? Fuzzy matching? Autocomplete?
"Integrate with X" → Read-only? Bidirectional sync? Real-time?

### Cross-repo blindspots
Check the memory file for repo relationships. Tickets often omit cross-repo impact because the reporter only knows their part of the system.

## Question Writing Guidelines

- Write for the ticket reporter, who may not be technical
- Provide concrete options (A/B/C) to reduce back-and-forth
- Explain WHY you're asking (impact on implementation) in one sentence
- Group related questions together
- Maximum 10 questions — prioritize by implementation impact
