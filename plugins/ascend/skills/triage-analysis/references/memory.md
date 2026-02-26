# Ascend Triage Memory

This file is the plugin's long-term memory. Edit it to teach the triage system about your organization's domain, repositories, conventions, and terminology.

All agents and skills in the ascend plugin read this file for context. Keep it up to date as your projects evolve.

---

## Domain Glossary

Define terms that have specific meanings in your organization. These definitions override generic interpretations during triage.

<!-- Add your terms below. Example format:
- **Widget**: The customer-facing dashboard component, not the generic UI term. Lives in `frontend-app/src/components/Widget/`.
- **Pipeline**: Our data processing pipeline in the `data-pipeline` repo, not CI/CD.
- **Tier 1 / Tier 2**: Tier 1 = self-serve customers. Tier 2 = enterprise customers with SLAs.
-->

_No terms defined yet. Add your domain-specific terms above._

---

## Repository Map

Define how your repositories relate to each other. This helps agents understand cross-repo impact.

<!-- Add your repos below. Include BOTH local path AND GitHub identifier so the system can fall back to GitHub when a repo isn't cloned locally.

Example format:

### frontend-app
- **GitHub**: `ascend-digital/frontend-app`
- **Local path**: `~/projects/frontend-app`
- **Purpose**: Customer-facing React SPA
- **Depends on**: api-gateway, shared-types
- **Owned by**: Frontend team
- **Key paths**: `src/components/`, `src/api/`, `src/store/`
-->

_No repositories defined yet. Add your repo map above._

---

## Team Conventions

Document conventions that affect how triage and specs are written.

<!-- Add your conventions below. Example format:
- **Branching**: GitFlow. Feature branches off `develop`, hotfixes off `main`.
- **PR reviews**: Minimum 2 approvals required. Security-sensitive changes need AppSec review.
- **Testing**: 80% coverage minimum. E2E tests required for user-facing features.
- **Naming**: Components use PascalCase. API endpoints use kebab-case.
- **Estimation**: Story points using Fibonacci. 1 point ≈ half day.
-->

_No conventions defined yet. Add your team conventions above._

---

## Known Technical Debt

Document areas of technical debt that affect triage estimation and risk assessment.

<!-- Add known debt below. Example format:
- **Auth module** (`api-gateway/src/auth/`): Legacy session-based auth. Migration to JWT planned for Q3. Any auth-related tickets carry extra risk.
- **Database migrations**: No rollback strategy. Schema changes are high-risk.
- **Test coverage gap**: `billing-service/src/invoicing/` has <30% coverage. Budget extra testing time for changes here.
-->

_No technical debt documented yet. Add known debt areas above._

---

## Entitlement Map

Maps the Jira "Entitlement" field (a product domain) to the corresponding repository. The triage system reads this map to automatically determine which repo a ticket belongs to.

If triage encounters an entitlement not listed here, it will **ask the user** for the correct repository and **add the mapping below** so it never has to ask again.

| Entitlement (domain) | GitHub Repositories | Notes |
|----------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uk.tradeshutters.com | ascend-digital/uk-trade-shutters | UK Trade Shutters portal |
| affiliates.shutterlyfabulous.com | ascend-digital/tcmm-affiliate-booking | Shutterly Fabulous affiliates booking, can have wildcard e.g. johnlewis.shutterlyfabulous.com which is shows an appointment request page, is built with Laravel. |
| mtm.mydecora.co.uk | ascend-digital/decora-m2m-portal | Decora MTM Portal for ordering blinds / made to measure products, deeply linked with Blindata. |

---

## Jira Project Context

Document Jira-specific context that helps with classification and routing.

## Atlassian Configuration
- **Jira site**: `ascend-agency.atlassian.net` (NOT ascend-digital)
- The Atlassian MCP server requires valid auth — if 401 errors occur, credentials need refreshing

## Entitlement Custom Fields
- `customfield_10744` — The structured entitlement object containing entitlementId, product (with name e.g. mtm.mydecora.co.uk), and entitledEntity (organization ID and type)
- `customfield_10932` — A simple string field containing the entitlement domain (e.g., "affiliates.shutterlyfabulous.com"). May be null — always check `customfield_10744` as fallback.

---

## Service Desk Custom Fields

Maps Jira custom field IDs to their purpose. These are the structured form fields that
Service Desk tickets use to capture reporter responses. The triage system reads this section
to know which fields to fetch — avoiding the need for a full-field discovery call on every run.

When triage discovers new fields, it adds them here automatically.

### Project: CS (Ascend Customer Service Desk)

**Bug report form fields:**

| Field ID | Purpose | Notes |
|---|---|---|
| `customfield_10777` | Reporter's description of the problem | Main content field — contains the user's explanation of what went wrong. May include inline images. |
| `customfield_10778` | What happened instead / actual behaviour | The user's description of unexpected behaviour |
| `customfield_10780` | Scope of impact | Select field. Values: "Most or all users", "Some users", etc. |
| `customfield_10781` | Severity self-assessment | Select field. Values: "It's inconvenient but important user journeys are still working", etc. |
| `customfield_10782` | Workaround availability | Select field. Values: "No, there is no workaround", "Yes, there is a workaround", etc. |
| `customfield_10130` | Priority classification | Auto-assigned priority (e.g., "P3 - Medium") |

**Template/boilerplate fields (skip these — they contain only unfilled templates):**

| Field ID | Purpose | Notes |
|---|---|---|
| `customfield_10100` | Feature request template | Contains "Please give a brief description of the feature" prompts — skip if only `-` answers |
| `customfield_10101` | Change request template | Contains "What feature would you like to change?" prompts — skip if only `-` answers |
| `customfield_10102` | Bug report template (alternative) | Contains "Please give a brief description of the problem" prompts — skip if only `-` answers |
| `customfield_10446` | Project overview template | Epic-level fields: Stakeholders, Goal, Target Users — usually empty on bugs |
| `customfield_10447` | Discovery template | Discovery inputs/outputs checklist — usually empty on bugs |
| `customfield_10448` | Development template | Development objectives template — usually empty on bugs |
| `customfield_10449` | User story template | AS A / I WANT / SO THAT template — usually empty on bugs |
| `customfield_10479` | Internal bug description template | Bug Description, Repro Steps, Expected/Actual result — skip if only `-` answers |
| `customfield_10314` | Additional notes | Often contains just "N/A" |
| `customfield_10316` | User story (alternative) | AS A / I WANT / SO THAT — usually unfilled |
