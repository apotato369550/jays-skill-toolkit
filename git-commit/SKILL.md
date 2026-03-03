---
name: git-commit
description: Handle git operations — committing, branching, merging, pushing, and rebasing. Invoke when the user asks to commit, push, branch, merge, or manage version control.
---

Execute git operations using the git-commit-haiku agent.

## Your Role

Invoke the `git-commit-haiku` agent via the Task tool. Pass the requested operation and any relevant context. The agent verifies state, executes, and reports — stopping immediately on conflicts or ambiguity.

## How to Invoke

1. **Identify the operation** from user's phrasing:
   - "commit my changes" → stage and commit
   - "push" → verify tracking branch, then push
   - "merge X into Y" → branch merge operation
   - "create a branch" → branch creation
   - "rebase" → rebase operation

2. **Gather context before launching**:
   - Any commit message the user specified (use it verbatim)
   - Which files to stage (specific files vs all changes)
   - Source and target branches for merge/rebase operations
   - Any flags explicitly requested (force-with-lease, rebase on pull, etc.)

3. **Launch the agent** with a prompt that includes:
   - The exact operation requested
   - Commit message if provided, or instruction to infer from diff
   - Files to stage if specified, otherwise stage relevant tracked changes
   - Any additional constraints or flags the user mentioned

## Prompt Template for Agent

```
Execute the following git operation: [operation]

Details:
- Commit message: [user-specified message | infer from diff]
- Files to stage: [specific files | all changes]
- Branch context: [current branch, target branch if relevant]
- Flags: [any explicit flags]

Verify state first. Stop immediately on conflicts, unclear state, or ambiguity.
```

## After Agent Completes

Report back cleanly:
```
Operation: [what was executed]
Result: [success / stopped]
[commit hash + message | branch name | merge status]
```

If the agent stopped due to conflict or unclear state, surface the block and next steps directly — do not re-attempt without user direction.
