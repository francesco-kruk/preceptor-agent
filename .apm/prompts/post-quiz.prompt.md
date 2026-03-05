---
description: Workflow that must run after every quiz answer is submitted in a Preceptor session
agent: Preceptor
---

After the user submits an answer to any quiz, always run this workflow without exception.

## Step 1 — Review answer

Use #tool:agent to run a subagent with the task of reviewing the quiz outcome:

- Provide it the question, all options, the correct answer, and the answer the user selected.
- Instruct it to determine whether the answer is correct, and — if incorrect — to identify the exact wrong belief the chosen option reflects (not just the topic). Ask it to describe the misconception as a single, precise statement of the false belief (e.g. "Believed that `===` coerces types before comparing").
- The subagent should return a structured result: whether the answer was correct, and the misconception string if the answer was incorrect.

## Step 2 — Record misconception (if answer is incorrect)

Use #tool:agent to run a second subagent tasked with updating `learning/WEAKNESSES.md`:

- Provide it the exact misconception string from Step 1.
- Instruct it to read the current contents of `learning/WEAKNESSES.md` using #tool:read (if it exists), then append the new entry to the `## Misconceptions` section following the structure defined in the [weaknesses schema](../context/weaknesses-schema.context.md), and write the updated file using #tool:edit.
- If `learning/WEAKNESSES.md` does not yet exist, instruct it to create it with the full scaffolding defined in the [weaknesses schema](../context/weaknesses-schema.context.md) before appending.

## Step 3 — Correct understanding (if answer is incorrect)

Do not simply reveal the correct answer. Target the specific misconception identified in Step 1 and guide the learner toward the correct understanding using one or more of the following approaches, calibrated to how far off the answer was:

- Ask targeted Socratic questions that surface the flaw in their reasoning.
- Provide a focused explanation that directly addresses the specific wrong belief.
- Present a new follow-up quiz that probes the same concept from a different angle, using the recorded misconception as a distractor to test whether the correction held.

Continue this correction loop — question, explain, re-quiz — until the learner demonstrates that the misconception has been resolved. Once resolved, use #tool:agent to run a subagent tasked with removing the now-resolved entry from `learning/WEAKNESSES.md`:

- Provide it the exact misconception string that was resolved.
- Instruct it to read the current contents of `learning/WEAKNESSES.md` using #tool:read, remove the bullet that matches the resolved misconception from the `## Misconceptions` section, and write the updated file using #tool:edit.
- The subagent must only remove the entry for the specific resolved misconception — all other entries must remain untouched.

Then proceed with the original learning path.

## Step 4 — Acknowledge and reinforce (if answer is correct)

Briefly affirm the correct answer, reinforce the underlying concept in one or two sentences, and proceed.
