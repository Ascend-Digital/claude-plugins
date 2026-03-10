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

**The single rule: if it can change during routine maintenance, it does not belong in the
wiki.** The wiki must never become a second place to update when code changes. If a developer
renames a class, adds an enum value, moves a file, or changes a route — the wiki should not
need touching.

Specifically, never include:

- **Code walkthroughs** — the code is the source of truth for implementation
- **Method signatures or class names** — these change and become stale
- **Step-by-step implementation guides** — that's what the code and its tests express
- **Copy-pasted code blocks** — unless essential for understanding data shapes or config
- **Enum or constant listings** — don't list every enum value, fabric code, or status constant.
  Describe the concept ("Frame types control how dimensions are calculated") not the catalogue.
- **File paths and Blade partials** — don't list view paths, partial includes, or directory
  trees. These change on every refactor and become stale immediately.
- **Database schema or metadata key tables** — don't document every column, JSON key, or
  metadata field. Describe what the data represents at a business level.
- **Eager-load chains or relationship listings** — don't list `->with(['a', 'b', 'c'])` chains.
  Describe the domain relationships instead.
- **PHP/JS implementations** — never paste the actual function body. If the logic is
  interesting, describe the *behaviour* in prose: "The manufacturing height adds sill modifiers
  to the ordered height when the frame type requires it."
- **Route lists or URL tables** — routes change constantly. Describe what endpoints exist
  conceptually ("a web UI for single orders and a bulk API for batch generation") not the
  exact paths.
- **Tech stack version numbers** — "Laravel 10", "PHP 8.2", "MySQL 8.0" all become wrong on
  the next upgrade. Say "Laravel" not "Laravel 10". If a version is critical to a decision,
  put it in Key Decisions with the reasoning.
- **Directory structure trees** — folder layouts change with every refactor. Describe the
  architectural layers ("services handle business logic, controllers handle HTTP") not the
  file tree.

**The litmus test:** before writing any detail, ask: "Would a developer need to update this
wiki line after a routine PR?" If yes, leave it out. The wiki documents *why the system is
designed this way*, not *what the code looks like today*.

Use code blocks ONLY for: environment variable names (not values), CLI commands, or API
request/response shapes that form a stable contract. When in doubt, leave the code out.

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

High-level explanation of behaviour from the user's or system's perspective. Use:
- Mermaid sequence/flow diagrams for processes
- ASCII box diagrams for architecture
- Tables for decision matrices or field mappings
- Numbered steps for sequential flows

Do NOT include implementation code, file paths, or class names. Describe what the system
does, not how the code is structured. A reader should understand the behaviour without
needing to see the codebase.

## Key Decisions

**This section is mandatory.** Every feature involves decisions worth capturing. For each
significant decision:
- What was decided
- What alternatives were considered
- Why this option was chosen (trade-offs accepted)

Even a two-line decision ("We chose X over Y because Z") is valuable. At 200+ projects,
these sections are the primary reason the wiki exists — they capture context that no amount
of code reading can recover.

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

## Pre-Commit Checklist

Before finishing, verify every article passes these checks. Do NOT skip this step.

1. **Frontmatter present** — every `.md` file has `---\ntitle: ...\n---` at the top
2. **Overview section exists** — first section after the H1 is `## Overview` with 2-3 sentences
3. **Key Decisions section exists** — at least one decision documented per article
4. **No code blocks containing implementation** — no PHP, JS, or framework code pasted from
   the codebase. Only env var names, CLI commands, or API shapes are acceptable.
5. **No file paths or class names** — the article should not reference specific files, Blade
   partials, service classes, or directory structures. Describe behaviour, not code layout.
6. **No enum/constant catalogues** — don't list every value of an enum or every key in a
   config. Describe the concept and its purpose.
7. **Routine maintenance test** — re-read every line and ask: "Would a developer need to
   update this after a routine PR (rename, refactor, version bump, route change)?" If yes,
   remove it. The wiki must not become a second place to maintain.
8. **No version numbers** — don't pin framework, language, or dependency versions unless the
   version is central to a Key Decision.
9. **Consistent section headings** — use the required headings: Overview, How It Works, Key
   Decisions, Failure Scenarios, Related
10. **Cross-links valid** — every Related section has at least one link; paths are relative
11. **Business language** — a non-developer should understand the Overview and Key Decisions
    sections. Technical depth belongs in How It Works and Failure Scenarios only.

