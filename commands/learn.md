---
name: learn
description: Start or resume learning a technical topic. Creates a workspace and curriculum if new, or picks up where you left off.
user_invocable: true
argument: topic name (e.g., "docker", "kubernetes", "go")
---

# /levelUp:learn <topic>

The user wants to learn `$ARGUMENTS`.

## Instructions

1. Read `~/.claude-level-up/config.json` to check if this topic already exists.
   - Validate each entry has `topic`, `workspace`, `started_at`, and `last_active` fields.
   - If any entry is malformed (missing fields or wrong types), attempt repair:
     - If `workspace` exists and contains `levelup_state/progress.json`, reconstruct the entry from that file.
     - Otherwise, remove the entry and warn the user.
   - Write the repaired array back to `config.json` before proceeding.

2. **If resuming an existing topic:**
   - Read `<workspace_dir>/levelup_state/progress.json` to find their current position.
   - Briefly summarize where they left off: current module, state, and what's next.
   - Continue from the current state (TEACHING, EXERCISE, or REVIEW).

3. **If starting a new topic:**
   - Create the workspace directory at `~/.claude-level-up/<topic>/`.
   - Create `~/.claude-level-up/<topic>/levelup_state/` directory.
   - Before writing, confirm no existing entry has the same `topic` value.
   - Add an entry to `config.json`:
     ```json
     {
       "topic": "<topic>",
       "workspace": "~/.claude-level-up/<topic>/",
       "started_at": "<ISO 8601 now>",
       "last_active": "<ISO 8601 now>"
     }
     ```
   - Initialize `progress.json` with the topic name, state set to `PERSONA_SELECTION`, empty modules array.
   - Ask the user to pick a teaching persona. Present the four options:
     - **Balanced** (default) — mix of explanation and challenge
     - **Drill Sergeant** — terse, demanding, maximum doing
     - **Lecturer** — thorough, patient, detailed explanations
     - **Socratic** — guides through questions, never explains directly
   - Also ask for their preferred coding language for building applications in exercises (e.g., a web server to deploy, a service that exports metrics). Clarify that this does **not** change how the tool itself is taught — tools are always taught using their native interface (CLI, UI, DSL, query language).

4. After persona selection, delegate to the `curriculum-planner` skill to research and build the curriculum.

5. Once the curriculum is ready, save it and begin teaching Module 1.

Use the `instructor` agent to manage all state transitions.
