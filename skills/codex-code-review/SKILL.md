---
name: codex-code-review
description: Review code changes using OpenAI Codex CLI. Use when the user asks for a code review, wants to check for bugs or security issues, or wants feedback on their changes before committing or merging.
---

# Codex CLI Code Review

AI-powered code review using OpenAI Codex CLI. Enables autonomous review workflows — implement a feature, review it with Codex, and fix issues without leaving your agent session.

## When to Use

When the user asks to:

- Review code changes / Review my code / Review this PR
- Check for bugs or security issues
- Find problems in my changes
- Run a code quality check
- Get feedback before merging
- Review staged or uncommitted changes

## How to Review

### 1. Check Prerequisites

```bash
# Check CLI is installed
codex --version 2>/dev/null || echo "NOT_INSTALLED"
```

**If not installed**, tell the user:

> Codex CLI is not installed. Install it with:
>
> ```bash
> npm install -g @openai/codex
> ```
>
> Then authenticate with your ChatGPT account by running `codex`, or set an API key:
>
> ```bash
> export OPENAI_API_KEY=sk-...
> ```

### 2. Run Codex Review

Use the built-in `codex review` subcommand. It automatically detects what to review and saves the full output to `.codex-review-output.md`.

```bash
# Review staged changes (default)
codex review

# Review all local changes (staged + unstaged + untracked)
codex review --uncommitted

# Review changes against a specific base branch
codex review --base main

# Review a specific commit
codex review --commit <sha>
```

You can also pass an optional prompt to focus the review on specific concerns:

```bash
codex review "focus on security vulnerabilities and performance issues"
codex review --base main "check for breaking API changes"
```

### 3. Present Results

Group findings by severity and create a task list for actionable issues:

```
## Codex Review Results

### 🔴 Critical
- [ ] `src/auth/login.ts:42` — SQL injection risk: user input not sanitised before query

### 🟠 High
- [ ] `src/api/users.ts:87` — Async function missing `await`, may return undefined

### 🟡 Medium
- [ ] `src/utils/format.ts:15` — Duplicated logic also present in `src/utils/parse.ts:30`

### 🟢 Low
- [ ] `src/components/Button.tsx:5` — Unused import `React` (not needed with new JSX transform)
```

### 4. Fix Issues (Autonomous Workflow)

When the user requests implementation + review:

1. Implement the requested feature
2. Run the Codex review (step 3)
3. Build a task list from findings
4. Fix each issue using Codex or directly
5. Re-run review if critical issues were found

## Documentation

- [Codex CLI](https://developers.openai.com/codex/cli)
- [AGENTS.md guide](https://developers.openai.com/codex/guides/agents-md)
