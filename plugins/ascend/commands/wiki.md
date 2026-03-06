---
description: "Write or update wiki documentation for the current project. Reads existing wiki content, explores the codebase, and writes articles to docs/wiki/ for automatic sync to the central wiki. Use after building new features or changing existing ones."
---

# Write Wiki Documentation

Write or update internal wiki articles for the current repository. Articles are written to
`docs/wiki/` and automatically synced to the central Ascend wiki.

## Input

`$ARGUMENTS` describes what to document, e.g.:

- `the new voucher system`
- `update basket docs to reflect delivery options`
- `architecture overview for this project`

If no arguments are provided, ask the user what they'd like to document.

## Workflow

### Step 1: Check Existing Wiki Content

Use the Ascend Wiki MCP to understand what's already documented for this project:

1. `list_projects` — confirm if this project has existing docs
2. `get_wiki_structure` — understand the structural layout and where articles sit
3. If the project has docs, use `read_section` with the project path to load all existing
   articles
4. `search_wiki` or `find_related_pages` with keywords from `$ARGUMENTS` to find related
   articles across all projects

Note which articles exist, what they cover, and what cross-links they have.

### Step 2: Explore the Codebase

Understand the feature or area being documented:

1. Check the repository structure — README, directory layout, key folders
2. Read relevant source files to understand the feature's behaviour, purpose, and design
3. Look at recent commits or changes if the documentation relates to a recent change
4. Identify external dependencies, integrations, and related systems

Focus on understanding **what** the system does and **why** — not cataloguing every file.

### Step 3: Decide — Create or Update

- **New feature with no existing article** → Create a new article
- **Existing article that's now stale** → Update the existing article
- **No `docs/wiki/` directory** → Create it with an `index.md` landing page first
- **`docs/wiki/` exists but project not yet in central wiki** → Treat as a new project; ensure
  `index.md` exists before writing feature articles
- **Broad request (e.g., "document this project")** → Create multiple articles following the
  standard directory structure from the skill guidelines

### Step 4: Write the Article

Follow the wiki-writing skill guidelines for article structure, content principles, and file
conventions.

### Step 5: Cross-Link

1. Add a "Related" section at the bottom of the new/updated article with links to related
   articles
2. Use `search_wiki` or `find_related_pages` to identify existing articles that should link
   back to the new article — update their "Related" sections in the local `docs/wiki/` files
3. Use correct relative paths as defined in the skill guidelines

### Step 6: Confirm

Tell the user:
- Which files were created or updated
- A brief summary of what was documented
- Any suggested follow-up articles that could be written
