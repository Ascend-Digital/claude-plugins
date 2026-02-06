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

### api-gateway
- **GitHub**: `ascend-digital/api-gateway`
- **Local path**: `~/projects/api-gateway`
- **Purpose**: Express.js API serving frontend and mobile
- **Depends on**: user-service, billing-service, shared-types
- **Owned by**: Backend team
- **Key paths**: `src/routes/`, `src/middleware/`, `src/models/`

### shared-types
- **GitHub**: `ascend-digital/shared-types`
- **Local path**: `~/projects/shared-types`
- **Purpose**: TypeScript type definitions shared across repos
- **Consumed by**: frontend-app, api-gateway, mobile-app
- **Owned by**: Platform team
- **Deploy note**: Changes here require coordinated releases
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

<!-- Add your entitlement-to-repo mappings below. Example format:
| Entitlement (domain) | Repository | GitHub | Notes |
|---|---|---|---|
| `uk.tradeshutters.com` | `uk-trade-shutters` | `ascend-digital/uk-trade-shutters` | UK Trade Shutters storefront |
| `us.example-brand.com` | `us-example-brand` | `ascend-digital/us-example-brand` | US Example Brand storefront |
-->

| Entitlement (domain) | Repository                       | GitHub | Notes                                                                                                                                                            |
|----------------------|----------------------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uk.tradeshutters.com | uk-trade-shutters | ascend-digital/uk-trade-shutters       | UK Trade Shutters portal                                                                                                                                         |
| affiliates.shutterlyfabulous.com | tcmm-affiliate-booking | ascend-digital/tcmm-affiliate-booking | Shutterly Fabulous affiliates booking, can have wildcard e.g. johnlewis.shutterlyfabulous.com which is shows an appointment request page, is built with Laravel. |

---

## Jira Project Context

Document Jira-specific context that helps with classification and routing.

<!-- Add your Jira context below. Example format:
- **Project key**: PROJ
- **Board**: Kanban board "Engineering"
- **Sprint cadence**: 2-week sprints, starting Mondays
- **SLA targets**: P1 = 4hr response, P2 = 1 business day, P3 = 1 sprint, P4 = backlog
- **Labels meaning**: `customer-facing` = affects end users. `internal` = tooling/infra.
-->

## Atlassian Configuration
- **Jira site**: `ascend-agency.atlassian.net` (NOT ascend-digital)
- The Atlassian MCP server requires valid auth — if 401 errors occur, credentials need refreshing

## Entitlement Custom Fields
- customfield_10744 — The structured entitlement object containing entitlementId, product (with name affiliates.shutterlyfabulous.com), and entitledEntity (organization ID 2 / TCMM)
- customfield_10932 — A simple string field containing "affiliates.shutterlyfabulous.com" (likely the "Domain Reporting" or display field)