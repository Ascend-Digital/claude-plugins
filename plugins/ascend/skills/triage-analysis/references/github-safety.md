# GitHub MCP Safety Rules

## READ-ONLY CONSTRAINT — NON-NEGOTIABLE

This plugin connects to GitHub via a **read-only SSE endpoint**. Two independent safety layers enforce this:

### Layer 1: Server-Side (SSE Endpoint)
The SSE endpoint itself should only expose read-only operations. Configure your GitHub MCP server to use read-only toolsets when starting it.

### Layer 2: Plugin-Side (allowedTools Whitelist)
The `.mcp.json` in this plugin contains an explicit `allowedTools` list. Even if the server exposes write tools, Claude Code will refuse to call anything not on this list.

## Allowed Tools (Read-Only)

### Repository browsing
- `get_file_contents` — read file content at a path and ref
- `search_repositories` — search for repos by query
- `search_code` — search code across repos
- `list_branches` — list branches in a repo
- `list_commits` — list commits on a branch
- `get_commit` — get details of a specific commit
- `get_repository` — get repo metadata
- `list_repository_files` — list files in a directory

### Issues (read-only)
- `get_issue` — read a single issue
- `list_issues` — list issues with filters
- `search_issues` — search issues across repos
- `get_issue_comments` — read comments on an issue

### Pull requests (read-only)
- `get_pull_request` — read PR details
- `list_pull_requests` — list PRs with filters
- `get_pull_request_files` — list files changed in a PR
- `get_pull_request_diff` — get the diff of a PR
- `get_pull_request_comments` — read PR comments
- `get_pull_request_reviews` — read PR reviews

### Releases (read-only)
- `list_tags` — list repo tags
- `list_releases` — list releases
- `get_release` — get details of a specific release

## EXPLICITLY BLOCKED — Never call these

Even if they appear available, these are **write operations** and must never be invoked:

- `create_issue`, `update_issue`, `add_issue_comment`
- `create_pull_request`, `merge_pull_request`, `update_pull_request`
- `create_branch`, `create_tag`, `create_release`
- `push_files`, `create_or_update_file`
- `create_repository`, `fork_repository`
- `add_pull_request_review`, `add_pull_request_comment`
- Any tool not listed in the Allowed Tools section above

## Agent Instructions

All agents in this plugin MUST:
1. Only use GitHub MCP tools listed in the Allowed Tools section
2. Never attempt to create, update, delete, or modify anything on GitHub
3. If a workflow would benefit from a write operation (e.g., posting a comment), output the content locally and let the human decide whether to post it
4. If an unknown GitHub tool is encountered, do NOT call it — default to refusing
