# Triage Quality Checklist

Used by the reviewer agent to validate triage report quality.

## Section 1: Understanding of the Request

- Does the summary accurately reflect ALL ticket content (including custom fields)?
- Were all content-bearing Jira fields read, not just `summary` and `description`?
- Is the goal clearly stated?
- Is the scope correctly identified (not too broad, not too narrow)?
- Are constraints and dependencies mentioned where applicable?
- Is context from comments, custom fields, and linked issues incorporated?
- Is the language business-friendly (non-technical)?
- Are there any invented requirements beyond what the ticket says?
- Are source fields noted (which Jira fields the information came from)?

## Section 2: How the System Currently Works

- Do referenced file paths actually exist in the codebase?
- Is the architecture description accurate and verifiable against code?
- Are diagrams helpful and correct (not just decorative)?
- Is the data flow traceable through actual code?
- Can a non-developer follow the explanation?
- Are cross-repo dependencies noted where applicable?
- Are external system boundaries clearly stated?
- Is the explanation focused on the areas relevant to the ticket?

## Section 3: Clarifying Questions

### Necessity
- Each question addresses a genuine ambiguity
- No question is already answered by the ticket's custom fields, comments, or attachments
- Memory glossary was checked (don't ask about defined terms)
- Codebase analysis was considered (don't ask what code already answers)

### Clarity
- Questions are readable by non-technical reporters
- Answer options are provided where possible
- "Why this matters" is explained in one sentence

### Prioritisation
- Questions ranked by implementation impact
- No more than 10 questions
- Related questions grouped together
- No duplicate or overlapping questions

## Cross-Section Consistency

- "Understanding" and "How the System Works" agree on scope
- Clarifying questions don't contradict assumptions in other sections
- File paths and components are consistent across all sections
- Memory definitions are applied consistently

## Process Concerns

- Were all content-bearing Jira fields discovered and read?
- Is the ticket completeness tier (A/B/C) accurate?
- Was exploration depth proportional to ticket specificity?
- If key terms yielded 0 codebase results, is the external system boundary noted?
- Are exploration gaps and limitations acknowledged?
- Does the report's confidence level match the available information?
