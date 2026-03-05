# Preceptor Mode

A Socratic coding coach and mentor that guides learners to discover answers themselves through questioning, explanation, and active tracking of their learning journey.

Rather than handing out code and commands, Preceptor walks you through the *why* at every step — asking guiding questions, quizzing your understanding, and building a persistent record of your misconceptions and weaknesses across sessions.

## Features

- **Socratic questioning** — asks open-ended, guiding questions before revealing answers or writing code
- **Step-by-step explanations** — explains the logic and rationale before generating any code
- **Command narration** — presents and justifies every terminal command before running it, with an explicit confirmation step
- **Adaptive quizzes** — tests understanding with plausible distractors targeting your recorded misconceptions; correct answer position is randomised by executing code at quiz time, preventing the predictable bias of LLM-generated "shuffles"
- **Learning tracking** — maintains an evolving `learning/WEAKNESSES.md` across sessions, adding new misconceptions as they surface and removing them once the learner proves they've been resolved
- **Session logs** — writes a journal of each session to `learning/logs/<date>-<sessionID>.md`
- **Learning data protection** — ensures `learning/` is excluded from version control automatically

## Installation

### Via APM (recommended)

[APM](https://github.com/microsoft/apm) is an open-source package manager for AI agent configuration.

**1. Install the APM CLI**

```sh
curl -sSL https://raw.githubusercontent.com/microsoft/apm/main/install.sh | sh
```

Or via Homebrew or pip — see the [APM getting started guide](https://github.com/microsoft/apm/blob/main/docs/getting-started.md).

**2. Add Preceptor to your project**

```sh
cd /path/to/your/project
apm install francesco-kruk/preceptor-mode
apm compile
```

This integrates the Preceptor agent into your project's agent configuration (`.github/` for Copilot, `.claude/` for Claude, etc.).

### Manual installation

If you prefer not to use APM, copy the agent file directly into your project:

```sh
mkdir -p .apm/agents
cp path/to/preceptor-mode/.apm/agents/Preceptor.agent.md .apm/agents/
```

## Usage

Once installed, switch to the **Preceptor** agent mode in your IDE, or invoke it with:

```
@Preceptor <your coding question or problem>
```

Preceptor works with GitHub Copilot (VS Code), Claude, Cursor, Codex, and any agent runtime that supports the `.agent.md` format.

## Learning Data

Preceptor automatically creates a `learning/` directory in your project root:

```
learning/
├── WEAKNESSES.md         # Evolving record of active misconceptions and struggles — entries are added when gaps are detected and removed once the learner demonstrates they've been resolved
└── logs/
    └── <date>-<id>.md    # Per-session learning journal
```

This directory is added to `.gitignore` automatically to keep your learning data private.

## Package Structure

```
.apm/
├── agents/
│   └── Preceptor.agent.md            # Agent persona, rules, and directives
├── prompts/
│   ├── pre-execute.prompt.md         # Workflow: explain every command before running it
│   ├── pre-quiz.prompt.md            # Workflow: prepare and present a quiz
│   └── post-quiz.prompt.md           # Workflow: review quiz answers and record misconceptions
└── context/
    └── weaknesses-schema.context.md  # Schema for learning/WEAKNESSES.md
```

The agent file is a slim entry point that links out to the prompt and context primitives. This means you can inspect or override any individual workflow without touching the agent persona itself.
