# codex-bridge

Claude Code skill for Codex CLI-powered codebase exploration and deep reasoning.

## Skills

### spark-explore

Codebase exploration using `gpt-5.3-codex-spark`. Runs in a read-only sandbox as a separate process, consuming zero main context.

- File pattern search, code keyword search, codebase structure/logic analysis
- Parallel execution of multiple queries for complex exploration
- Automatically replaces the Explore subagent via SessionStart hook

### using-oracle

Deep technical analysis using `gpt-5.1-codex-max`. Runs in a read-only sandbox as a separate process.

- Complex architecture design and trade-off analysis
- Security/performance code review
- Hard debugging after 2+ failed fix attempts
- Strategic technical advisory and refactoring roadmaps

## Prerequisites

- [Codex CLI](https://github.com/openai/codex) (`npm i -g @openai/codex`)
- Authenticated via `codex login`

## Usage

Skills are invoked automatically by Claude Code based on context. To invoke directly:

```
/spark-explore "Find all authentication middleware and trace the request validation flow"
```

```
/using-oracle "Should we split this monolith into microservices? The codebase has 50k LOC."
```

## License

MIT
