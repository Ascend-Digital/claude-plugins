---
description: "Register a repository path for multi-repo triage. Adds a repo to the memory file so the triage system knows where to look during cross-repo investigation."
---

# Register Repository

Register a local repository path so the triage system can investigate it during cross-repo analysis.

## Input

`$ARGUMENTS` should contain the repo details in this format:
```
<name> <local-path> [description]
```

Examples:
- `/ascend:register-repo api-gateway ~/projects/api-gateway "Express.js API layer"`
- `/ascend:register-repo frontend-app /home/user/projects/frontend "React SPA"`

## Workflow

1. Parse the arguments to extract: repo name, local filesystem path, and optional description.
2. Verify the path exists and is a valid git repository (check for `.git/` directory).
3. Auto-discover repo metadata:
   - Read package.json / pyproject.toml / Cargo.toml / go.mod for tech stack
   - Read README first paragraph for description (if user didn't provide one)
   - List top-level directories for key paths
   - Check for common dependency patterns (imports, shared packages)
4. Update `references/memory.md` in the plugin's skill directory, adding or updating the repo entry in the Repository Map section.
5. Confirm registration to the user with a summary of what was discovered.

## Multi-Repo Triage Flow

When the `/ascend:triage` command runs, the orchestrator:
1. Reads the Repository Map from memory
2. Analyzes the ticket to determine which repos are likely relevant
3. Explores EACH relevant repo (not just the current working directory)
4. The spec-generator and clarifier agents receive cross-repo context

This means you can run triage from ANY repo directory and it will still investigate all registered repos that are relevant to the ticket.
