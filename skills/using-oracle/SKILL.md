---
name: using-oracle
description: Use when facing complex architectural decisions, deep technical analysis, strategic recommendations, code review requiring elevated reasoning, or after 2+ failed fix attempts on hard debugging problems. Make sure to use this skill whenever the user asks for architecture advice, trade-off analysis, security or performance review, refactoring strategy, or any question that benefits from deep deliberate reasoning — even if they don't explicitly ask for an "oracle" or "second opinion".
allowed-tools: mcp__plugin_codex-bridge_codex__codex, mcp__plugin_codex-bridge_codex__codex-reply
---

# Deep Reasoning with Codex Max

Uses gpt-5.4 via Codex MCP server in a read-only sandbox. Runs as a separate process, consuming zero main context.

## Decision Guide

Answer directly when the question is simple or it's your first attempt at a fix. Reach for this skill when you need elevated reasoning — complex trade-offs, stubborn bugs after 2+ failed attempts, unfamiliar code patterns, or security/performance concerns.

**Avoid when:**
- Simple file operations (use direct tools)
- First attempt at any fix (try yourself first)
- Questions answerable from code you've already read
- Trivial decisions (variable names, formatting)

## Execution

Read `${CLAUDE_PLUGIN_ROOT}/skills/using-oracle/references/developer-instructions.md`, then call the `mcp__plugin_codex-bridge_codex__codex` tool:

```json
{
  "prompt": "<analysis query>",
  "model": "gpt-5.4",
  "sandbox": "read-only",
  "developer-instructions": "<contents of developer-instructions.md>"
}
```

- Use `$ARGUMENTS` as the `prompt` if provided; otherwise formulate the query yourself based on context.
- **Include context**: When the query involves code, include relevant code snippets, file paths, or `git diff` output directly in the prompt. The richer the context, the better the analysis.
- For complex analysis, make multiple parallel MCP tool calls with different focused queries rather than one broad query.
- To continue a conversation, use `mcp__plugin_codex-bridge_codex__codex-reply` with the `threadId` from the previous response.

**Good queries:**
- "Should we split this monolith into microservices? The codebase has 50k LOC with tightly coupled modules."
- "Review this authentication implementation for security concerns: $(git diff HEAD~1)"
- "Compare event-driven vs request-response architecture for our real-time notification system."

**Bad queries:**
- "What should we do?" (too vague)
- "Review my code" (no context provided)

Present the output directly to the user.

## Error Handling

- Auth error -> tell user to run `codex login`
- `command not found` -> tell user to install Codex CLI (`npm i -g @openai/codex`)
- Empty/unhelpful output -> reformulate with more specific context
