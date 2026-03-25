# Claude Codex Plugin

OpenAI Codex CLI integration for Claude Code.

Run Codex CLI tasks — code review, generation, refactoring, and more — directly from your Claude Code session.

## Installation

### 1. Prerequisites

Install and authenticate the Codex CLI:

```bash
# Install via npm
npm install -g @openai/codex

# Install via Homebrew (macOS)
brew install --cask codex
```

Authenticate with one of:

```bash
# ChatGPT account (Plus / Pro / Team)
codex   # select "Sign in with ChatGPT" in the interactive UI

# OpenAI API key
export OPENAI_API_KEY=sk-...
```

### 2. Install Plugin

Inside the Claude Code chat interface, run:

```
/plugin marketplace add hirokazumiyaji/claude-codex-plugin
/plugin marketplace update
/plugin install claude-codex-plugin@claude-codex-plugin-marketplace
```

Or via the Claude CLI:

```bash
claude plugin marketplace add hirokazumiyaji/claude-codex-plugin
claude plugin marketplace update
claude plugin install claude-codex-plugin@claude-codex-plugin-marketplace
```

If you manage marketplaces manually, you can also add this repository itself as a marketplace:

```bash
claude plugin marketplace add hirokazumiyaji/claude-codex-plugin
```

> For full Claude Code plugin documentation, see the [Claude Code plugin guide](https://docs.claude.ai/en/docs/claude-code/plugins).

## Usage

### Run a Codex task

```
/codex <task description>
```

Examples:

```
/codex Review the current changes for bugs and security issues
/codex Write unit tests for src/auth/login.ts
/codex Refactor the `processOrder` function to reduce complexity
/codex Fix the failing test in tests/api.test.ts
```

### Natural Language

You can also ask Claude naturally — the Codex agent will be used automatically:

- "Use Codex to review my changes"
- "Ask Codex to write tests for this module"
- "Run a Codex code review"

## How It Works

1. Claude receives your request
2. The plugin checks that Codex CLI is installed and authenticated
3. Claude invokes Codex CLI with a well-scoped prompt
4. Codex runs in `full-auto` mode (or a mode you specify) and applies or proposes changes
5. Claude presents the results grouped by type and severity

## Skills

This plugin ships with cross-platform skills that work with Codex CLI and other supported agents:

| Skill | Description |
|-------|-------------|
| `codex-code-review` | Review code changes for bugs, security issues, and quality problems |

Install skills directly to Codex CLI:

```bash
# Project-level
cp -r skills/codex-code-review .codex/skills/

# Global
cp -r skills/codex-code-review ~/.codex/skills/
```

Or use the `skills` package manager (if installed):

```bash
npx skills add hirokazumiyaji/claude-codex-plugin
```

## Resources

- [Codex CLI Documentation](https://developers.openai.com/codex/cli)
- [AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md)
- [Codex CLI GitHub](https://github.com/openai/codex)
- [Reference: CodeRabbit Claude Plugin](https://github.com/coderabbitai/claude-plugin)

## License

MIT
