---
name: spark-explore
description: Use INSTEAD of the Explore subagent for all codebase exploration and deep research tasks. Make sure to use this skill whenever the user asks to explore a codebase, trace code flows, understand architecture, find files by patterns, search code for keywords, or answer questions about codebase structure and logic — even if they don't explicitly mention "explore". If a task requires more than 2 search queries to answer, this skill is the right choice.
allowed-tools: mcp__plugin_codex-bridge_codex__codex, mcp__plugin_codex-bridge_codex__codex-reply
---

# Codebase Exploration with Codex Spark

Uses gpt-5.3-codex-spark via Codex MCP server in a read-only sandbox. Runs as a separate process, consuming zero main context.

## Decision Guide

Use Glob/Grep directly when you already know the file path or need 1-2 simple searches. Use this skill for everything else — multi-step exploration, tracing flows across files, understanding architecture, or any question that requires reading and connecting multiple parts of the codebase.

## Execution

Read `${CLAUDE_PLUGIN_ROOT}/skills/spark-explore/references/developer-instructions.md`, then call the `mcp__plugin_codex-bridge_codex__codex` tool:

```json
{
  "prompt": "<exploration query>",
  "model": "gpt-5.3-codex-spark",
  "sandbox": "read-only",
  "developer-instructions": "<contents of developer-instructions.md>"
}
```

- Use `$ARGUMENTS` as the `prompt` if provided; otherwise formulate the query yourself based on context.
- For complex exploration, make multiple parallel MCP tool calls with different focused queries rather than one broad query.
- To continue a conversation, use `mcp__plugin_codex-bridge_codex__codex-reply` with the `threadId` from the previous response.

**Good queries:**
- "Find all authentication middleware and trace the request validation flow"
- "List files handling database connections and explain the pooling strategy"
- "What patterns are used for error handling across src/api/?"
- "Map the data flow from user signup to email verification"

**Bad queries:**
- "explore the codebase" (too vague)
- "find files" (no specifics — use Glob instead)

Present the output directly to the user.

## Error Handling

- Auth error -> tell user to run `codex login`
- `command not found` -> tell user to install Codex CLI (`npm i -g @openai/codex`)
- Empty/unhelpful output -> reformulate with more specific terms, or fall back to Glob/Grep
