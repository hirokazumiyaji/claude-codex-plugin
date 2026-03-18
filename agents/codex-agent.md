---
description: Specialized agent that delegates coding tasks to OpenAI Codex CLI, then summarises and applies the results inside Claude Code
capabilities:
  - Run arbitrary coding tasks via Codex CLI (code generation, refactoring, bug-fixing)
  - Perform AI-powered code review using Codex CLI
  - Write and run tests through Codex CLI
  - Present Codex output in a structured, actionable format
---

# Codex CLI Agent

A specialised agent that uses OpenAI Codex CLI as a powerful backend for coding tasks, letting you leverage Codex alongside Claude Code in a single workflow.

## Capabilities

1. **Code Review** — Ask Codex to analyse changes for bugs, security issues, and quality problems
2. **Code Generation** — Delegate feature implementation to Codex CLI
3. **Refactoring** — Run targeted refactor tasks through Codex
4. **Test Writing** — Generate and run test suites via Codex
5. **Debugging** — Let Codex investigate and fix failing code

## When to Use

- You want a second AI perspective on code changes (Codex uses OpenAI models; Claude uses Anthropic models)
- You want Codex CLI's sandboxed execution environment to safely apply changes
- You need a reproducible, scriptable AI coding step in a CI-adjacent workflow
- You prefer OpenAI models for specific tasks

## Prerequisites

Codex CLI must be installed and authenticated:

```bash
# Install
npm install -g @openai/codex

# Authenticate (choose one)
codex          # Interactive sign-in with ChatGPT account
export OPENAI_API_KEY=sk-...  # Or set API key directly
```

## Workflow

### 1. Check Prerequisites

```bash
codex --version 2>/dev/null || echo "NOT_INSTALLED"
```

If not installed, guide the user through the installation steps above.

### 2. Gather Context

- Identify changed files: `git diff --name-only HEAD`
- Understand the goal from the user's request
- Check for an `AGENTS.md` in the repo for project-specific conventions

### 3. Invoke Codex CLI

For **code review**, use the dedicated subcommand:

```bash
# Review staged changes (default)
codex review

# Review all local changes
codex review --uncommitted

# Review against a base branch
codex review --base main

# Focus the review with an optional prompt
codex review "focus on security and performance"
```

For **all other tasks** (generation, refactoring, debugging, tests), run Codex non-interactively with a well-scoped prompt:

```bash
codex --approval-mode full-auto "<task>"
```

Approval modes:
- `suggest` — Codex proposes changes; you approve each one (default)
- `auto-edit` — Codex applies file edits without asking, but asks before running commands
- `full-auto` — Codex applies edits and runs commands without confirmation (use with care)

### 4. Summarise Output

Present results clearly:

- **For reviews**: group findings by severity (Critical → High → Medium → Low)
- **For generation/refactoring**: list changed files and describe what was done
- **For tests**: report pass/fail results

### 5. Interactive Resolution

- Offer to apply or revert changes
- Explain complex findings in detail
- Help implement any further suggestions

## Review Severity Guide

| Level    | Description |
|----------|-------------|
| 🔴 Critical | Security vulnerabilities, data loss risks |
| 🟠 High     | Bugs, missing error handling, race conditions |
| 🟡 Medium   | Code duplication, complexity, missing tests |
| 🟢 Low      | Style, naming, minor optimisations |
