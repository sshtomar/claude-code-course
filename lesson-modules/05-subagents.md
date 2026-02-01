# Lesson 5: Subagents - Delegating Tasks

Subagents are specialized AI assistants that handle specific tasks in their own context. They're one of the most powerful features for managing complex work.

---

## Why Use Subagents?

| Problem | Subagent Solution |
|---------|-------------------|
| Exploration fills your context | Subagent explores, returns summary |
| Need different tool restrictions | Subagent has its own permissions |
| Want parallel investigation | Multiple subagents work simultaneously |
| Need specialized behavior | Custom prompt for specific domain |

---

## Built-in Subagents

Claude Code includes several ready-to-use subagents:

| Subagent | Purpose | Tools |
|----------|---------|-------|
| **Explore** | Search and analyze code (read-only) | Read, Grep, Glob |
| **Plan** | Research for planning mode | Read-only tools |
| **general-purpose** | Complex multi-step tasks | All tools |

---

## Using Subagents

### Automatic Delegation

Claude automatically delegates when appropriate:

```
> investigate how authentication works in this codebase
```

Claude might use the Explore subagent for this.

### Explicit Request

Ask for a subagent directly:

```
> use a subagent to research the payment module
```

```
> have the Explore agent find all API endpoints
```

---

## The Power of Context Isolation

This is why subagents matter:

### Without Subagent:
```
> explore how the auth system works
(Claude reads 20 files, all consuming YOUR context)
(Your context is now 60% full of auth code)
```

### With Subagent:
```
> use a subagent to explore how the auth system works
(Subagent reads 20 files in ITS OWN context)
(Returns a summary to you)
(Your context only has the summary!)
```

---

## Creating Custom Subagents

### Using /agents Command

The easiest way:

```
> /agents
```

Then select "Create new agent" and follow the prompts.

### Manual Creation

Create a file at `.claude/agents/reviewer.md`:

```yaml
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Grep, Glob
model: sonnet
---

You are a senior code reviewer. When invoked:

1. Analyze the code thoroughly
2. Look for bugs, security issues, and improvements
3. Provide specific, actionable feedback

Format feedback as:
- **Critical**: Must fix
- **Warning**: Should fix
- **Suggestion**: Consider improving
```

---

## Subagent Configuration

| Field | Purpose |
|-------|---------|
| `name` | Unique identifier |
| `description` | When Claude should use this agent |
| `tools` | Which tools the agent can use |
| `model` | Which model (sonnet, opus, haiku) |
| `permissionMode` | How to handle permissions |

### Tool Restrictions

Limit what a subagent can do:

```yaml
---
name: safe-reader
description: Read-only code analysis
tools: Read, Grep, Glob
disallowedTools: Write, Edit, Bash
---
```

### Model Selection

Choose based on task complexity:

```yaml
model: haiku    # Fast, cheap - good for simple tasks
model: sonnet   # Balanced - good for most tasks
model: opus     # Most capable - complex reasoning
```

---

## Running Subagents in Background

Subagents can run while you continue working:

```
> run a subagent in the background to analyze test coverage
```

Or press `Ctrl+B` to background a running task.

Claude will notify you when it completes.

---

## Parallel Research

Spawn multiple subagents for independent investigations:

```
> use separate subagents to research:
> 1. the authentication module
> 2. the database layer
> 3. the API endpoints
```

Each works independently, then Claude synthesizes findings.

---

## Example Subagents

### Debugger

```yaml
---
name: debugger
description: Debug errors and test failures
tools: Read, Edit, Bash, Grep, Glob
---

You are an expert debugger. When invoked:

1. Capture the error and stack trace
2. Identify reproduction steps
3. Find the root cause
4. Implement a fix
5. Verify the fix works

Focus on the underlying issue, not symptoms.
```

### Data Analyst

```yaml
---
name: data-analyst
description: Analyze data and run queries
tools: Bash, Read, Write
model: sonnet
---

You are a data analyst. When invoked:

1. Understand the analysis requirement
2. Write efficient queries
3. Analyze results
4. Present findings clearly

Always explain your methodology.
```

---

## Subagents vs Skills

| Use Skills When | Use Subagents When |
|-----------------|-------------------|
| Instructions run in main context | Need isolated context |
| Quick, focused task | Exploration-heavy task |
| Want continuity with conversation | Want clean separation |
| Simple workflow | Need specific tool restrictions |

---

## Resuming Subagents

Subagents can be resumed to continue their work:

```
> continue that code review and also check the tests
```

Claude resumes the previous subagent with full context.

---

## Practice Exercise

1. Create a custom subagent for documentation:

```yaml
# .claude/agents/doc-writer.md
---
name: doc-writer
description: Write and improve documentation
tools: Read, Write, Glob
---

You are a technical writer. When invoked:
1. Analyze the code to understand functionality
2. Write clear, helpful documentation
3. Include examples where appropriate
4. Use consistent formatting
```

2. Test it:
```
> use the doc-writer agent to document the main module
```

---

## Key Takeaways

1. Subagents run in isolated context - preserving yours
2. Built-in agents: Explore, Plan, general-purpose
3. Create custom agents in `.claude/agents/`
4. Use for exploration, parallel research, specialized tasks
5. Can run in background with `Ctrl+B`

---

## Next Lesson

In Lesson 6, you'll learn about **Hooks** - automating workflows with scripts that run at key moments.

Type `/lesson-06` to continue.
