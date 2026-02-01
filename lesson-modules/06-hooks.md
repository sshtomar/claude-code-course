# Lesson 6: Hooks - Workflow Automation

Hooks are shell commands that run automatically at specific points in Claude's workflow. Unlike CLAUDE.md instructions (which are suggestions), hooks are **deterministic** - they always run.

---

## Why Use Hooks?

| Situation | Hook Solution |
|-----------|---------------|
| Must format code after every edit | PostToolUse hook runs Prettier |
| Must never edit certain files | PreToolUse hook blocks the edit |
| Want notification when done | Notification hook sends alert |
| Need to inject context after compact | SessionStart hook adds reminders |

---

## Hook Events

Hooks can fire at these lifecycle points:

| Event | When It Fires |
|-------|---------------|
| `SessionStart` | Session begins, resumes, or compacts |
| `PreToolUse` | Before a tool runs (can block it) |
| `PostToolUse` | After a tool completes |
| `Notification` | When Claude needs attention |
| `Stop` | When Claude finishes responding |
| `SessionEnd` | Session terminates |

---

## Creating Your First Hook

### Using /hooks Menu

The easiest way:

```
> /hooks
```

Select an event, add your command.

### Manual Configuration

Add to `.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'File was edited!'"
          }
        ]
      }
    ]
  }
}
```

---

## Matchers: Filtering When Hooks Run

Matchers filter which events trigger your hook:

| Event | What Matcher Filters |
|-------|---------------------|
| `PreToolUse`, `PostToolUse` | Tool name |
| `SessionStart` | How session started |
| `Notification` | Notification type |

### Examples

```json
{
  "matcher": "Bash",
  "matcher": "Edit|Write",
  "matcher": "mcp__github__.*"
}
```

An empty matcher `""` matches everything.

---

## Example: Auto-Format After Edits

Run Prettier on every file Claude edits:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ]
  }
}
```

The hook receives JSON input via stdin with details about the tool call.

---

## Example: Desktop Notifications

Get notified when Claude needs input:

**macOS:**
```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude needs attention\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

**Linux:**
```json
{
  "command": "notify-send 'Claude Code' 'Claude needs attention'"
}
```

---

## Example: Block Protected Files

Prevent edits to sensitive files:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/protect-files.sh"
          }
        ]
      }
    ]
  }
}
```

The script (`.claude/hooks/protect-files.sh`):

```bash
#!/bin/bash
INPUT=$(cat)
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

# Block certain patterns
if [[ "$FILE_PATH" == *".env"* ]] || [[ "$FILE_PATH" == *"package-lock"* ]]; then
  echo "Blocked: Cannot edit $FILE_PATH" >&2
  exit 2  # Exit 2 = block the action
fi

exit 0  # Exit 0 = allow
```

---

## Exit Codes

Your hook tells Claude what to do via exit code:

| Exit Code | Meaning |
|-----------|---------|
| `0` | Proceed (stdout added to context for some events) |
| `2` | Block the action (stderr sent to Claude as feedback) |
| Other | Proceed (stderr logged, not shown to Claude) |

---

## Hook Input Data

Hooks receive JSON on stdin with event details:

```json
{
  "session_id": "abc123",
  "cwd": "/path/to/project",
  "hook_event_name": "PreToolUse",
  "tool_name": "Bash",
  "tool_input": {
    "command": "npm test"
  }
}
```

Parse with `jq`:
```bash
INPUT=$(cat)
COMMAND=$(echo "$INPUT" | jq -r '.tool_input.command')
```

---

## Hook Location

Where you store hooks determines scope:

| Location | Scope |
|----------|-------|
| `~/.claude/settings.json` | All your projects |
| `.claude/settings.json` | This project (shareable) |
| `.claude/settings.local.json` | This project (private) |

---

## Prompt-Based Hooks

For decisions requiring judgment, use a Claude model:

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Check if all tasks are complete. Return {\"ok\": false, \"reason\": \"what remains\"} if not."
          }
        ]
      }
    ]
  }
}
```

The model returns `{"ok": true}` or `{"ok": false, "reason": "..."}`.

---

## Agent-Based Hooks

For verification requiring file access:

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "agent",
            "prompt": "Verify all tests pass. Run the test suite and check results.",
            "timeout": 120
          }
        ]
      }
    ]
  }
}
```

An agent can read files and run commands to verify conditions.

---

## Common Hook Patterns

### Re-inject Context After Compaction

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Remember: use Bun not npm. Run tests before commits.'"
          }
        ]
      }
    ]
  }
}
```

### Log All Bash Commands

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.command' >> ~/.claude/command-log.txt"
          }
        ]
      }
    ]
  }
}
```

---

## Practice Exercise

Create a hook that runs ESLint after JavaScript edits:

1. Add to `.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(cat | jq -r '.tool_input.file_path'); if [[ \"$FILE\" == *.js ]] || [[ \"$FILE\" == *.ts ]]; then npx eslint --fix \"$FILE\"; fi"
          }
        ]
      }
    ]
  }
}
```

2. Ask Claude to edit a JavaScript file
3. Verify ESLint ran automatically

---

## Key Takeaways

1. Hooks run deterministically - not suggestions
2. Use matchers to filter when hooks fire
3. Exit 0 to allow, exit 2 to block
4. Hooks receive JSON input via stdin
5. Use prompt/agent hooks for judgment calls

---

## Next Lesson

In Lesson 7, you'll learn about **Plan Mode** and advanced workflows for complex tasks.

Type `/lesson-07` to continue.
