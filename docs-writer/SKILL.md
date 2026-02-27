---
name: docs-writer
description: Update project documentation based on available context. Invoke when the user asks to sync, update, or write docs. Adapts to ARCHITECTURE.md, git diffs, or explicit instructions as source of truth.
---

Update project documentation using the docs-sync-manager agent.

## Your Role

Invoke the `docs-sync-manager` agent via the Task tool. Pass the context and target files. The agent updates the docs and confirms what changed.

## How to Invoke

1. **Identify target files** from user's phrasing:
   - Explicit: `"update the README"` → `README.md`
   - General: `"sync the docs"` → infer from what files exist (README.md, CLAUDE.md, CHANGELOG.md)

2. **Identify source of truth** in priority order:
   - ARCHITECTURE.md (if present and stated as current) → pass it
   - Git diff → run `git diff` or `git diff [base]...HEAD` and pass the output
   - Explicit orchestrator instructions → pass user's description directly

3. **Launch the agent** with a prompt that includes:
   - Which files to update
   - The source context (ARCHITECTURE.md content, git diff, or explicit instructions)
   - Any specific constraints or style notes the user mentioned

## Prompt Template for Agent

```
Update the following documentation files: [file list]

Source context:
[ARCHITECTURE.md content | git diff output | explicit instructions]

Additional notes: [any user constraints]
```

## After Agent Completes

Report back to the user with what changed:
```
Docs updated:
- README.md: [brief description of changes]
- CHANGELOG.md: [brief description of changes]
```

No editorializing. Mirror the agent's confirmed output cleanly.
