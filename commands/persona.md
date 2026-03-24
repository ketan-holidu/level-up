---
name: persona
description: Set or change your teaching persona. Options are balanced, drill-sergeant, lecturer, or socratic.
user_invocable: true
argument: persona name (balanced, drill-sergeant, lecturer, socratic)
---

# /levelUp:persona [name]

The user wants to set or change their teaching persona to `$ARGUMENTS`.

## Instructions

1. Read `~/.claude-level-up/config.json` to find the active topic.
   - If `config.json` is empty or contains no valid entries, tell the user no topic is active and suggest `/levelUp:learn <topic>`.
   - Validate each entry has `topic`, `workspace`, `started_at`, and `last_active` fields. Skip entries missing required fields.
   - If multiple valid topics exist, select the one with the most recent `last_active` timestamp.

2. **If a persona name was provided:**
   - Validate it's one of: `balanced`, `drill-sergeant`, `lecturer`, `socratic`.
   - If invalid, list the valid options with brief descriptions.
   - If valid, update `progress.json` with the new persona.
   - Confirm the change and briefly describe how the teaching style will shift.

3. **If no persona name was provided:**
   - Show the current persona.
   - Present all four options with descriptions:
     - **Balanced** — concise explanations alternating with probing questions. Adapts depth based on your responses.
     - **Drill Sergeant** — terse definitions, minimal examples, immediate tasks. No hand-holding.
     - **Lecturer** — detailed explanations from first principles. Analogies, line-by-line code walkthroughs, full coverage.
     - **Socratic** — teaches entirely through questions. Guides you to discover concepts yourself.
   - Ask the user to pick one.

4. If no topic is active yet, save the persona preference and note it will be applied when they start learning.
