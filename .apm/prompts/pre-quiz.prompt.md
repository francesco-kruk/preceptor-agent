---
description: Workflow that must be completed before presenting any quiz in a Preceptor session
agent: Preceptor
---

Every quiz presented using #tool:vscode/askQuestions MUST follow this workflow in full. Do not skip or collapse any step. There are NO exceptions.

## Step 1 — Check recorded weaknesses

Use #tool:read to read `learning/WEAKNESSES.md` (if it exists). Identify any recorded misconceptions or struggles that are relevant to the concept being quizzed.

- If no weaknesses file exists yet, proceed without any recorded misconceptions.
- Keep the relevant entries in context for Step 3.

## Step 2 — Confirm the question targets the learning objective

Before composing the quiz, verify that the question being asked directly tests the concept you are currently teaching or probing. The question must be precise, unambiguous, and answerable with a single correct option.

## Step 3 — Craft realistic distractors

Generate the incorrect answer options. Distractors must be plausible and challenging — never obviously wrong:

- **Prioritise recorded misconceptions**: If Step 1 surfaced relevant misconceptions from `learning/WEAKNESSES.md`, design one or more distractors that directly embody those wrong beliefs. This makes the quiz personally targeted to the learner's known gaps.
- **Fall back to common misconceptions**: If no relevant recorded misconceptions exist, infer the most common wrong beliefs newcomers hold about the concept and use those as distractors.
- Each distractor must reflect a specific, believable wrong belief — not a vague or obviously absurd alternative.

## Step 4 — Randomise option order

Do **not** rely on your own internal ordering to shuffle options — LLM outputs are not random and will produce statistically predictable patterns (typically placing the correct answer first). Instead, use #tool:execute to run the following one-liner, which produces a genuine random position for the correct answer:

```bash
python3 -c "import random; print(random.randint(0, 3))"
```

The integer printed (0 = first slot, 1 = second, 2 = third, 3 = fourth) is the 0-based index at which the correct answer must be placed. Fill the remaining slots, in any order, with the distractors.

If the quiz has a number of options other than 4, adjust the upper bound accordingly (e.g. `random.randint(0, 2)` for 3 options).

## Step 5 — Present the quiz

Use #tool:vscode/askQuestions to present the quiz:

- Set **no `hint`** on any option — hints reveal the answer and defeat the purpose.
- Include only the options composed in Steps 3–4.
- Do not editorialize or add commentary that could hint at the correct answer.

## Step 6 — Hand off to post-quiz workflow

After the user selects an answer, immediately follow the full [post-quiz workflow](../prompts/post-quiz.prompt.md) — without exception.
