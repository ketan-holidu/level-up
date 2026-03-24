---
name: review
description: Submit your exercise code for review. The code reviewer will evaluate your solution and give feedback.
user_invocable: true
---

# /levelUp:review

The user is submitting their exercise for review.

## Instructions

1. Read `~/.claude-level-up/config.json` to find the active topic.
   - If `config.json` is empty or contains no valid entries, tell the user to start with `/levelUp:learn <topic>`.
   - Validate each entry has `topic`, `workspace`, `started_at`, and `last_active` fields. Skip entries missing required fields.
   - If multiple valid topics exist, select the one with the most recent `last_active` timestamp.

2. Read the topic's `progress.json` to confirm the current state.

3. **If the state is EXERCISE:**
   - Ask the user where their code is (file path) if they haven't already indicated it in their message.
   - Read their submitted code.
   - Update state to `REVIEW`.
   - Delegate to the `code-reviewer` skill with:
     - The learner's code
     - The exercise requirements (from the current module in `curriculum.md`)
     - The persona preference
     - Previous module summaries for context
   - After the review, transition to ADVANCE:
     - Record the module summary
     - Mark the module as completed
     - Move to the next module or complete the curriculum

4. **If the state is not EXERCISE:**
   - Let the user know they're not currently in an exercise phase.
   - Tell them their current state and what to do next.
