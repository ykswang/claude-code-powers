---
name: "claude-code"
displayName: "Claude Code"
description: "A wrapper for Claude Code CLI MCP service for file operations and code editing"
keywords: ["claude", "code", "mcp", "cli", "edit", "file", "grep"]
author: "closeli"
---

# Claude Code MCP

## Overview

This Power wraps the Claude Code CLI MCP service, resolving compatibility issues between native `claude mcp serve` and Kiro, allowing you to use all Claude Code capabilities within Kiro.

**Important:** All file path parameters must use **absolute paths**. Relative paths are not supported.

## Available Steering Files

- **task-usage** - Guidelines for using Task tool, including requirement to specify working directory in prompts

## Prerequisites

**Install and configure Claude CLI**
```bash
# Check if installed
which claude
```

## Tool Reference

### 1. Read - Read File

Read local file content. Supports images, PDFs, and Jupyter notebooks.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| file_path | string | ✅ | Absolute file path |
| offset | number | ❌ | Starting line number |
| limit | number | ❌ | Number of lines to read (default 2000) |

### 2. Write - Write File

Write or overwrite file content.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| file_path | string | ✅ | Absolute file path |
| content | string | ✅ | File content |

### 3. Edit - Edit File

Edit file via string replacement.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| file_path | string | ✅ | Absolute file path |
| old_string | string | ✅ | Text to replace |
| new_string | string | ✅ | New text |
| replace_all | boolean | ❌ | Replace all matches (default false) |

### 4. NotebookEdit - Edit Jupyter Notebook

Edit cells in .ipynb files.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| notebook_path | string | ✅ | Absolute notebook path |
| new_source | string | ✅ | New cell content |
| cell_id | string | ❌ | Cell ID |
| cell_type | string | ❌ | "code" or "markdown" |
| edit_mode | string | ❌ | "replace", "insert", or "delete" |

### 5. Glob - File Pattern Search

Search files using glob patterns.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| pattern | string | ✅ | Glob pattern (e.g., `**/*.ts`) |
| path | string | ❌ | Search directory (default: current working directory) |

### 6. Grep - Content Search

Search file content using regex (powered by ripgrep).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| pattern | string | ✅ | Regular expression |
| path | string | ❌ | Search directory (default: current working directory) |
| glob | string | ❌ | File filter (e.g., `*.js`) |
| output_mode | string | ❌ | "content", "files_with_matches", "count" |
| -i | boolean | ❌ | Case insensitive |
| -n | boolean | ❌ | Show line numbers (default true) |
| -A | number | ❌ | Lines after match |
| -B | number | ❌ | Lines before match |
| -C | number | ❌ | Lines before and after match |
| type | string | ❌ | File type (js, py, rust, etc.) |
| head_limit | number | ❌ | Limit output lines |
| offset | number | ❌ | Skip first N lines |
| multiline | boolean | ❌ | Multiline matching mode |

### 7. Bash - Execute Command

Execute bash commands in a persistent shell session.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| command | string | ✅ | Command to execute |
| timeout | number | ❌ | Timeout in ms (max 600000, default 120000) |
| description | string | ❌ | Command description (5-10 words) |
| run_in_background | boolean | ❌ | Run in background |
| dangerouslyDisableSandbox | boolean | ❌ | Disable sandbox mode |

**Note:** No cwd parameter. Use absolute paths in commands.

### 8. KillShell - Terminate Background Shell

Terminate a background shell.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| shell_id | string | ✅ | Shell ID |

### 9. Task - Launch Agent Task

Launch autonomous agent for complex multi-step tasks.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| description | string | ✅ | Short description (3-5 words) |
| prompt | string | ✅ | Detailed task content |
| subagent_type | string | ✅ | Agent type |
| model | string | ❌ | Model: "sonnet", "opus", "haiku" |
| resume | string | ❌ | Resume from previous agent ID |
| run_in_background | boolean | ❌ | Run in background |

### 10. TaskOutput - Get Task Output

Retrieve output from background tasks.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| task_id | string | ✅ | Task ID |
| block | boolean | ❌ | Wait for completion (default true) |
| timeout | number | ❌ | Max wait time in ms (default 30000) |

### 11. WebFetch - Fetch Web Page

Fetch web content and process with AI.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| url | string | ✅ | URL (HTTP auto-upgraded to HTTPS) |
| prompt | string | ✅ | Prompt for content extraction |

### 12. WebSearch - Web Search

Search the web and return results.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| query | string | ✅ | Search query |
| allowed_domains | array | ❌ | Only search these domains |
| blocked_domains | array | ❌ | Exclude these domains |

### 13. TodoWrite - Task List

Create and manage task lists.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| todos | array | ✅ | Array of tasks |

Each task item:
- `content` (string): Task content
- `status` (string): "pending", "in_progress", "completed"
- `activeForm` (string): Present continuous description

### 14. AskUserQuestion - Ask User

Ask user questions for input.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| questions | array | ✅ | Array of questions (1-4) |

Each question:
- `question` (string): Question content
- `header` (string): Short label (max 12 chars)
- `options` (array): Options (2-4)
- `multiSelect` (boolean): Allow multiple selections

### 15. Skill - Execute Skill

Execute predefined skills.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| skill | string | ✅ | Skill name |
| args | string | ❌ | Skill arguments |

### 16. EnterPlanMode - Enter Plan Mode

Enter plan mode for complex task planning. No parameters.

### 17. ExitPlanMode - Exit Plan Mode

Exit plan mode and submit plan for user approval. No parameters.

## Troubleshooting

### spawn claude ENOENT

**Cause:** `claude` command not in PATH

**Solution:**
```bash
which claude
```

### Authentication Failed

**Solution:** Configure Claude CLI authentication according to your setup.

## Configuration

No manual configuration required after installing this Power.

If Claude CLI is not in PATH, add environment variable:
```json
{
  "mcpServers": {
    "claude-code": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "closeli-claudecode-mcp@latest"],
      "env": {
        "CLAUDE_PATH": "/your/path/to/claude"
      }
    }
  }
}
```

---

**npm package:** `closeli-claudecode-mcp`
