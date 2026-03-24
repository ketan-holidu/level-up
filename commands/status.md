---
name: status
description: Show current learning progress — completed modules, current position, and overall stats.
user_invocable: true
---

# /levelUp:status

The user wants to see their learning progress.

## Instructions

1. Read `~/.claude-level-up/config.json` to get the list of active topics.
   - Validate each entry has `topic`, `workspace`, `started_at`, and `last_active` fields.
   - Skip any entries missing required fields and warn the user about skipped entries.

2. If there are no valid topics, tell the user they haven't started learning anything yet and suggest `/levelUp:learn <topic>`.

3. If there's one topic, show its progress. If multiple, show a summary of each and ask which one they want details on.

4. For the selected topic, read `<workspace_dir>/levelup_state/progress.json` and display:
   - **Topic** and **Persona**
   - **Overall progress**: X of Y modules completed
   - **Current state**: what they're doing right now (learning, working on exercise, awaiting review)
   - **Module breakdown**: list each module with status (completed / in progress / upcoming)
   - For completed modules, show a one-line summary from the module's `summary` field
   - **Started** and **Last active** timestamps

Format this as a clean, readable summary. Use a simple table or structured list.
