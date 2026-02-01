# Lesson 1: Getting Started with Claude Code

Welcome to your first Claude Code lesson! By the end of this lesson, you'll understand what Claude Code is, how to navigate a session, and complete your first coding task.

---

## What is Claude Code?

Claude Code is an **agentic coding environment**. Unlike a chatbot that just answers questions, Claude Code can:

- Read and analyze your files
- Run terminal commands
- Make code changes
- Work autonomously through problems

Think of it as an AI pair programmer that lives in your terminal.

---

## Understanding the Interface

When you start Claude Code, you'll see:

```
╭─────────────────────────────────────────────╮
│  Claude Code                                │
│                                             │
│  Working directory: /path/to/your/project   │
│  Model: claude-sonnet-4-...                 │
╰─────────────────────────────────────────────╯
>
```

The `>` prompt is where you type your requests.

### Key Interface Elements

| Element | Description |
|---------|-------------|
| `>` prompt | Where you type requests |
| Status line | Shows model, tokens used, git branch |
| Tool calls | Displayed when Claude reads files, runs commands |

---

## Essential Commands

### Starting and Stopping

```bash
claude              # Start interactive mode
claude -c           # Continue most recent conversation
claude -r           # Resume a previous conversation
exit                # Exit (or Ctrl+C)
```

### Built-in Slash Commands

| Command | What it does |
|---------|--------------|
| `/help` | Show available commands |
| `/clear` | Clear conversation history |
| `/compact` | Summarize and compress context |
| `/config` | Open configuration |

**Try it now:** Type `/help` to see all available commands.

---

## Your First Interaction

Let's try some basic interactions. Type these prompts:

### 1. Ask About the Codebase
```
what does this project do?
```

Claude will analyze your files and provide a summary.

### 2. Ask Specific Questions
```
what technologies does this project use?
```

```
where is the main entry point?
```

### 3. Ask About Claude's Capabilities
```
what can you help me with?
```

---

## Making Your First Code Change

Now let's make Claude do some actual work:

```
create a file called hello.txt with the text "Hello from Claude Code!"
```

Claude will:
1. Show you what it plans to do
2. Ask for your permission
3. Create the file

**Important:** Claude always asks permission before modifying files.

---

## Permission System

When Claude wants to make changes, you'll see a permission prompt:

```
Claude wants to write to hello.txt
[y] Yes  [n] No  [a] Accept all
```

| Option | Meaning |
|--------|---------|
| `y` | Allow this one action |
| `n` | Deny this action |
| `a` | Accept all similar actions this session |

---

## Practice Exercise

Try these tasks to practice what you've learned:

1. **Explore:** Ask Claude "what files are in this directory?"
2. **Create:** Ask Claude to "create a simple Python hello world script"
3. **Explain:** Ask Claude to "explain what the script does"
4. **Modify:** Ask Claude to "add a function that says goodbye"

---

## Key Takeaways

1. Claude Code is an **agentic** tool - it can take actions, not just chat
2. Use natural language to describe what you want
3. Claude always asks permission before making changes
4. Use `/help` when you're unsure what's available

---

## Next Lesson

In Lesson 2, you'll learn about **context management** - the most important concept for using Claude Code effectively.

Type `/lesson-02` to continue.
