---
name: session-saver
description: Archive and summarize the current conversation session into a structured markdown report. Invoke when the user asks to save, archive, or capture a session. Supports brief (default) and verbose modes.
---

Archive the current conversation into a structured session report using the session-saver agent.

## Your Role

Invoke the `session-saver` agent via the Task tool. Pass the full conversation context. The agent writes a `.md` report and returns a completion summary.

## How to Invoke

1. **Detect mode** from user's phrasing:
   - Default → **brief** (~50-100 lines, scannable)
   - "verbose", "full details", "capture everything" → **verbose** (~150-300 lines)

2. **Detect descriptor** from user's phrasing:
   - Explicit: `"Archive as 'ASIN-Refactor'"` → use directly
   - Implicit: extract focus area from conversation (e.g., "Bug Fixes", "Qdrant Integration")
   - Fallback: use `"Session"` with today's date

3. **Launch the agent** with a prompt that includes:
   - The full conversation history (summarize if very long, but preserve key events)
   - Mode (brief or verbose)
   - Descriptor (explicit or inferred)
   - Today's date for filename construction (`MM-DD-YYYY`)

## Prompt Template for Agent

```
Archive this session in [BRIEF|VERBOSE] mode.

Descriptor: [descriptor]
Date: [MM-DD-YYYY]

Conversation context:
[paste or summarize the full session here]
```

## After Agent Completes

Report back to the user:
```
Session archived ([BRIEF|VERBOSE]) to MM-DD-YYYY_SESSION_descriptor.md

Key stats:
- Files modified: N
- Actions: M
- Status: [completion state]
```

Mirror the agent's output cleanly. No editorializing.
