---
description: Run a task with OpenAI Codex CLI directly from Claude Code
argument-hint: "<task description> [--model <model>] [--approval-mode <mode>]"
allowed-tools: Bash(codex:*), Bash(git:*)
---

# Codex CLI Run

Delegate a coding task to OpenAI Codex CLI and present the results.

## Context

- Current directory: !`pwd`
- Git repo: !`git rev-parse --is-inside-work-tree 2>/dev/null && echo "Yes" || echo "No"`
- Branch: !`git branch --show-current 2>/dev/null || echo "detached HEAD"`
- Changed files: !`git diff --name-only HEAD 2>/dev/null | head -20 || echo "none"`

## Instructions

Task to run with Codex: **$ARGUMENTS**

### Prerequisites Check

**Skip if already verified earlier in this session.**

Run:

```bash
codex --version 2>/dev/null || echo "NOT_INSTALLED"
```

**If Codex CLI is not installed**, tell the user:

> Codex CLI is not installed. Install it with one of:
>
> ```bash
> # npm (recommended)
> npm install -g @openai/codex
>
> # Homebrew (macOS)
> brew install --cask codex
> ```
>
> Then authenticate:
> - **ChatGPT account**: Run `codex` and select **Sign in with ChatGPT**
> - **API key**: Set `OPENAI_API_KEY` in your environment

**If `OPENAI_API_KEY` is missing and user is not using ChatGPT sign-in**, tell the user:

> Set your OpenAI API key before running Codex:
>
> ```bash
> export OPENAI_API_KEY=sk-...
> ```

### Run Codex Task

Once prerequisites are met, invoke Codex non-interactively with the given task.
If the user does not provide `--approval-mode`, you must use `full-auto`:

```bash
codex --approval-mode full-auto "$TASK"
```

Where `$TASK` comes from `$ARGUMENTS`. Common patterns:

- `codex --approval-mode full-auto "Review the current code changes for bugs and security issues"`
- `codex --approval-mode full-auto "Refactor the function in src/foo.ts to improve readability"`
- `codex --approval-mode full-auto "Write unit tests for src/bar.ts"`

Pass through any `--model` or `--approval-mode` flags specified in `$ARGUMENTS`.

### Present Results

Summarise what Codex did:

1. **Changes made** — list any files created or modified
2. **Findings** — if it was a review task, group by severity
3. **Next steps** — suggest follow-up actions if applicable
