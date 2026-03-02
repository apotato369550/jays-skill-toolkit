---
name: trace-and-investigate
description: Trace execution paths and isolate root causes. Read-only investigation — no code changes. Invoke when the user asks to debug, trace, or investigate system behavior.
---

Investigate and isolate the root cause of an issue using the debug-investigator agent.

## Your Role

Invoke the `debug-investigator` agent via the Task tool. Pass the symptom and any relevant context. The agent traces, inspects, and reports — it never modifies anything.

## How to Invoke

1. **Identify the symptom** from user's phrasing:
   - Memory leak → agent traces allocations, process state, open file descriptors
   - Unexpected behavior → agent traces execution paths and logic branches
   - Performance degradation → agent profiles expensive operations and bottlenecks
   - Orphaned files / redundant code → agent scans filesystem and grep patterns

2. **Identify scope** — narrow it before launching:
   - Which file, script, service, or process is involved?
   - Any known reproduction steps or error output the user has shared?
   - Are there relevant logs, config files, or state files to inspect?

3. **Launch the agent** with a prompt that includes:
   - The symptom as precisely as stated by the user
   - Scope (file path, process name, function, directory, etc.)
   - Any context already provided (error messages, logs, observations)

## Prompt Template for Agent

```
Investigate the following issue:

Symptom: [exact description of what's broken or unexpected]
Scope: [file / directory / process / service]

Context:
[any error output, logs, or prior observations the user shared]

Report: symptom, evidence, root cause, impact, and reproduction steps.
```

## After Agent Completes

Report findings back to the user in this compressed form:
```
Root cause: [one sentence]

Evidence: [key command output or code excerpt]

Impact: [effect on behavior / performance / correctness]

To reproduce: [minimal steps]
```

Signal high, noise low. If multiple issues were found, rank by severity. Do not propose fixes — the agent only investigates. Fixes belong to a separate delegation.
