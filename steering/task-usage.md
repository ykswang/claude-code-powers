---
inclusion: fileMatch
fileMatchPattern: "**/*"
---

# Task Tool Usage Guidelines

When using the **Task** or **TaskOutput** tools from Claude Code MCP:

## Always Include Current Directory in Prompt

Since the Task tool launches a subprocess agent that doesn't inherit the current working directory context, you **MUST** explicitly specify the working directory in the `prompt` parameter.

### Required Pattern

```json
{
  "tool": "Task",
  "description": "Search for tests",
  "prompt": "Working directory: /Users/username/project\n\nFind all test files and summarize their coverage.",
  "subagent_type": "default"
}
```

### Why This Matters

- The Task agent runs as an independent subprocess
- It has no knowledge of the parent's current working directory
- Without explicit directory context, the agent may operate in an unexpected location
- File paths in the agent's response may be incorrect or relative to wrong directory

### Best Practices

1. **Start prompt with working directory**: Begin your prompt with `Working directory: /absolute/path`
2. **Use absolute paths**: When referencing specific files, use absolute paths
3. **Be explicit about scope**: Clearly state which directories or files the agent should work with

### Example Prompts

**Good:**
```
Working directory: /Users/cc/projects/my-app

Analyze the React components in src/components and suggest performance improvements.
```

**Bad:**
```
Analyze the React components in src/components and suggest performance improvements.
```
