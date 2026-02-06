# Technical Specification Template

Use this template when generating specs. Adapt sections based on ticket type.

## For Bugs

```markdown
## Overview
{What is broken and the business impact}

## Reproduction Steps
1. {Step-by-step to reproduce}
2. ...
**Expected**: {what should happen}
**Actual**: {what happens instead}
**Environment**: {browser, OS, version, etc.}

## Root Cause Analysis
**Likely cause**: {file path and function, with reasoning}
**Confidence**: {High/Medium/Low}

## Fix Approach
### Option A (Recommended)
{Description, files affected, estimated effort}
### Option B
{Alternative approach if Option A has risk}

## Affected Files
| File | Change Type | Complexity |
|------|------------|------------|
| `path/to/file` | Modify | Simple |

## Acceptance Criteria
- [ ] Bug no longer reproducible following steps above
- [ ] Existing tests pass
- [ ] New regression test added for this scenario
- [ ] No performance degradation

## Risks
| Risk | Mitigation |
|------|-----------|
| {risk} | {mitigation} |
```

## For New Features

```markdown
## Overview
{What is being built and why}

## User Stories
- As a {role}, I want {action}, so that {benefit}

## Functional Requirements
1. {Requirement — concrete and testable}
2. ...

## Non-Functional Requirements
- Performance: {targets}
- Security: {considerations}
- Accessibility: {requirements}

## Architecture
### New Components
{What needs to be created}
### Modified Components
{What needs to change}
### Data Model Changes
{Schema additions/modifications with migration strategy}
### API Changes
{New or modified endpoints}

## Implementation Plan
| Step | Description | Dependencies | Complexity | Est. Time |
|------|------------|-------------|------------|-----------|
| 1 | {step} | None | Simple | 2h |

## Integration Points
- **Upstream**: {services this depends on}
- **Downstream**: {services that depend on this}
- **Cross-repo**: {other repos affected — reference memory}

## Acceptance Criteria
- Given {precondition}, When {action}, Then {result}

## Testing Strategy
- Unit: {what to test}
- Integration: {what to test}
- E2E: {scenarios}
- Edge cases: {list}

## Out of Scope
- {Explicitly excluded items}
```

## For Technical Support

```markdown
## Overview
{What the user is experiencing}

## Diagnostic Plan
1. {First thing to check}
2. {Second thing to check}
3. ...

## Root Cause (if identified)
{Cause and evidence}

## Resolution
### Immediate Fix
{Steps to resolve the current issue}
### Permanent Fix (if different)
{Long-term solution}

## Prevention
{How to prevent recurrence}

## Acceptance Criteria
- [ ] User's issue is resolved
- [ ] Root cause is addressed (or ticket created for permanent fix)
- [ ] Documentation updated if needed
```
