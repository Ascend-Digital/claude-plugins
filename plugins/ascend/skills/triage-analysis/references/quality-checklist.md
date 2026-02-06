# Triage Quality Checklist

Used by the reviewer agent to validate triage output quality.

## Classification Quality

### Type Classification
- Does the classified type match the ticket's actual intent?
- If the Jira issue type differs from classification, is the override justified?
- Are mixed-concern tickets handled (primary + noted secondary)?

### Severity Assessment
- Is severity based on actual impact, not just reporter urgency?
- Critical: Is there genuine data loss, security risk, or system outage?
- High: Is the workaround truly painful, not just inconvenient?
- Medium/Low: Would a reasonable PM agree with this ranking?

### Confidence Calibration
- Are confidence scores differentiated (not all 90%+)?
- Do low-confidence flags point to specific missing information?
- Would additional context actually change the classification?

## Spec Quality

### Traceability
- Every requirement links to something in the ticket
- No "invented" requirements beyond ticket scope
- Out-of-scope section catches potential creep

### Implementability
- File paths reference actual codebase locations
- Implementation steps are ordered with dependencies
- Complexity estimates are realistic for the codebase
- Another developer could implement from this spec alone

### Testability
- Acceptance criteria use concrete, verifiable conditions
- Given/When/Then format where applicable
- Edge cases are identified

### Cross-Repo Completeness
- Memory repo map was consulted
- Cross-repo impacts are noted in integration points
- Coordinated release requirements flagged if applicable

## Clarifying Questions Quality

### Necessity
- Each question addresses a genuine ambiguity
- Memory glossary was checked (don't ask about defined terms)
- Codebase analysis was considered (don't ask what code already answers)

### Clarity
- Questions are readable by non-technical reporters
- Answer options are provided where possible
- "Why we're asking" is explained in one sentence

### Prioritization
- Questions ranked by implementation impact
- No more than 10 questions
- Related questions grouped together
