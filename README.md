# codex-bridge

Claude Code plugin that brings OpenAI models into your workflow via [Codex CLI](https://github.com/openai/codex). Each skill runs in a read-only sandbox as a separate process, consuming zero main context.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Codex CLI](https://github.com/openai/codex)

```sh
npm i -g @openai/codex
codex login
```

## Installation

```sh
claude plugin add jylkim/codex-bridge
```

The Codex CLI MCP server is registered automatically on install.

## Skills

### spark-explore

Codebase exploration powered by `gpt-5.3-codex-spark`.

- File pattern search, code keyword search, codebase structure/logic analysis
- Parallel queries for complex multi-file exploration
- Replaces the built-in Explore subagent automatically

```
/spark-explore "Find all authentication middleware and trace the request validation flow"
```

**When to use:** Multi-file flow tracing, architecture discovery, or any exploration requiring 2+ search queries.

### using-oracle

Deep technical analysis powered by `gpt-5.4`.

- Architecture design and trade-off analysis
- Security/performance code review
- Hard debugging after 2+ failed fix attempts
- Refactoring strategy and roadmaps

```
/using-oracle "Should we split this monolith into microservices? The codebase has 50k LOC."
```

**When to use:** Complex architectural decisions, security/performance reviews, or debugging escalation after repeated failures.

## How It Works

Skills are invoked automatically by Claude Code based on context, or manually via slash commands. Each skill sends queries to OpenAI models through the Codex CLI MCP server and runs in a read-only sandbox — no code is modified.

```
skills/
├── spark-explore/    # Codebase exploration (gpt-5.3-codex-spark)
│   ├── SKILL.md
│   └── references/
└── using-oracle/     # Deep technical analysis (gpt-5.4)
    ├── SKILL.md
    └── references/
```

## License

MIT
