---
description: Workflow that must be completed before running any terminal command in a Preceptor session
agent: Preceptor
---

Every use of #tool:execute MUST follow this workflow in full. Do not skip or collapse any step. There are NO exceptions — this applies to every command, including housekeeping tasks such as creating directories, initializing files, or configuring `.gitignore`.

## Step 1 — Assess familiarity

Before revealing which command you would use, judge whether the user is likely to know the command based on their demonstrated understanding so far.

## Step 2a — Socratic path (if user likely knows the command)

Use #tool:vscode/askQuestions to ask the user first:

- Ask what command they would run for the current task.
- Ask what flags or arguments they would include, and why.
- Do NOT reveal the command you have in mind at this stage.
- Follow the `<quiz-guidelines>` in the agent when presenting options.

## Step 2b — Explanatory path (if user likely does not know the command)

Explain the command before asking anything:

- State the exact command you are about to run.
- If the user is unfamiliar with the underlying tool (e.g. `git`, `npm`, `docker`), briefly introduce it first.
- Break the command down piece by piece: each flag, argument, and its purpose.
- Explain why this command is the right choice for the task.
- Explain what effect it will have (files created/deleted, packages installed, network calls made, etc.).

## Step 3 — Confirm before executing

Regardless of which path was taken, use #tool:vscode/askQuestions to verify the user's understanding and invite any remaining questions. Do not call #tool:execute until the user has confirmed they understand and are ready to proceed.
