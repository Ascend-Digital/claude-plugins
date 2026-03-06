---
name: wiki-writing
description: "Write or update internal wiki documentation for Ascend projects. Automatically activates when new features are built, existing features are significantly changed, or architecture decisions are made. Reads the existing wiki via MCP, explores the codebase, and writes articles to the source repo's docs/wiki/ folder for automatic sync."
---

# Wiki Writing Skill

Write and maintain wiki documentation that explains **what** a system does and **why** it works
that way. The code already expresses implementation detail — the wiki captures intent, decisions,
behaviour, and failure modes that code cannot.

## When to Use This Skill

After completing any of the following, ALWAYS offer to write or update wiki documentation
before closing out the task. Do not auto-write — ask the user first:

> "Would you like me to write wiki documentation for [feature name]? I can create an article
> in docs/wiki/ that explains what it does and why it was designed this way."

Triggers:

- A **new feature** has been implemented — it needs documenting
- An **existing feature** has been significantly changed — existing docs may be stale
- **Architecture decisions** have been made — capture the reasoning before it's forgotten
- A **bug fix** revealed non-obvious behaviour — document it in the relevant article
- **Integration points** have changed — document how systems connect and why

## Content Principles

### What to Write

- **What** the feature/system does — behaviour, not implementation
- **Why** it exists — the business problem it solves
- **Why this approach** — what alternatives were considered, what trade-offs were accepted
- **What can go wrong** — failure scenarios and how the system handles them
- **How things connect** — relationships between features, services, and external systems

### What NOT to Write

- **Code walkthroughs** — the code is the source of truth for implementation
- **Method signatures or class names** — these change and become stale
- **Step-by-step implementation guides** — that's what the code and its tests express
- **Copy-pasted code blocks** — unless essential for understanding data shapes or config

Use code blocks only for: configuration examples, CLI commands, data structure shapes, or
environment variables.

### Tone and Style

- Write for a developer who is new to the project but technically competent
- Use plain language — avoid jargon unless defined in the project glossary
- Prefer diagrams (Mermaid, ASCII, tables) over long prose for flows and relationships
- Keep articles focused — one concept per article, split if it grows beyond ~800 words
- Use British English spelling (behaviour, colour, organisation)

## Article Structure

Every article MUST include these sections. Skip a section only if it genuinely does not apply.

### Required Sections

```markdown
---
title: Article Title
order: 1
---

# Article Title

## Overview

2-3 sentences: what this is and why it exists. A developer should understand the purpose
after reading just this section.

## How It Works

High-level explanation of behaviour. Use:
- Mermaid sequence/flow diagrams for processes
- ASCII box diagrams for architecture
- Tables for decision matrices or field mappings
- Numbered steps for sequential flows

Do NOT include implementation code. Describe behaviour from the user's or system's perspective.

## Key Decisions

Why the approach was chosen. For each significant decision:
- What was decided
- What alternatives were considered
- Why this option was chosen (trade-offs accepted)

## Failure Scenarios

What can go wrong and what happens when it does:
- Name the failure scenario
- Describe the user/system impact
- Explain how the system recovers (or doesn't)

## Related

Cross-links to related wiki articles. Use relative markdown links:
- Same directory: [Article Title](filename.md)
- Subdirectory: [Article Title](subfolder/filename.md)
- Parent directory: [Article Title](../filename.md)
- Different section: [Article Title](/other-section/filename.md)
```

### Optional Sections

Add these when relevant:

- **Edge Cases** — non-obvious behaviours, boundary conditions
- **Operational Considerations** — caching, monitoring, scheduled tasks, manual triggers
- **External Dependencies** — third-party APIs, services, what happens when they're down
- **Configuration** — environment variables, feature flags, config files (tables preferred)

## Cross-Linking Rules

Articles MUST link to related articles. This is critical for navigability at scale.

### Link Format

Use relative markdown links. The wiki is built with VitePress and synced from `docs/wiki/`:

```markdown
- Same directory: [Basket Architecture](basket.md)
- Subdirectory: [Payments](architecture/payments.md)
- Parent directory: [Getting Started](../getting-started.md)
- Root-level reference: [Operations](/other/operations.md)
```

### When to Link

- When mentioning a concept that has its own article
- In the "Related" section at the bottom of every article
- When a failure scenario references another system's behaviour

### Discovering Existing Articles

Before writing, ALWAYS use the wiki MCP to check what already exists:

1. `list_projects` — confirm if this project has existing docs
2. `get_wiki_structure` — understand the structural layout and where articles sit
3. `read_section` with the project path — load all existing articles for this project
4. `search_wiki` or `find_related_pages` — find articles across all projects that mention the
   feature you're documenting

After writing a new article, use `search_wiki` or `find_related_pages` with the new article's
topic to identify existing articles that should link back to it. Update their "Related" sections
in the local `docs/wiki/` files.

## File Conventions

### Location

Articles are written to `docs/wiki/` in the current repository (the source repo). The wiki
sync system automatically pulls them into the central wiki.

### Naming

- Kebab-case filenames: `voucher-system.md`, `cache-strategy.md`
- Subdirectories for grouping: `architecture/`, `integrations/`
- `index.md` at root for the project landing page

### Frontmatter

Every article MUST have YAML frontmatter:

```yaml
---
title: Human-Readable Title
order: 1
---
```

- `title` — displayed in the sidebar. Required.
- `order` — controls sidebar sort order. Lower numbers appear first. Optional.

### Directory Structure

Follow this standard layout across all projects:

```
docs/wiki/
  index.md                    # Project landing page
  getting-started.md          # Setup and local dev
  tech-stack.md               # Runtime versions and dependencies
  environments.md             # Environments and branching
  architecture/
    overview.md               # High-level architecture
    {feature}.md              # Per-feature architecture articles
  integrations/
    {service}.md              # External service integrations
  quirks.md                   # Non-obvious behaviours
```

Not every project needs every file. Create what's relevant, but follow the naming and
structure when you do.

