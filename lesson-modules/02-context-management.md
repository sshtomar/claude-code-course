# Lesson 2: Understanding Context Management

The most important concept in Claude Code is **context**. This lesson explains how context works and why managing it well is the key to getting great results.

---

## What is Context?

Context is Claude's working memory - everything Claude knows about your conversation:

- All messages you've sent
- All files Claude has read
- All command outputs
- Claude's own responses

Context has a **fixed size limit** (called the context window). When it fills up, older information gets summarized or forgotten.

---

## Why Context Matters

| More Context | Less Context |
|--------------|--------------|
| Claude remembers more details | Claude is faster and more focused |
| Risk of information overload | May forget earlier instructions |
| Longer response times | Quicker responses |

**Key insight:** Performance can degrade as context fills up. A fresh context with a good prompt often beats a long conversation with accumulated corrections.

---

## Checking Your Context

Look at your status line (bottom of terminal). It shows:
- Current token usage
- Percentage of context used

When you see context getting high (>70%), consider:
1. Running `/compact` to summarize
2. Running `/clear` to start fresh

---

## The `/clear` Command

Use `/clear` to reset your context between unrelated tasks:

```
> /clear
```

**When to use:**
- Switching to a completely different task
- After many corrections that polluted the context
- When Claude seems confused or inconsistent

**Don't use when:**
- You need continuity from the previous work
- You're in the middle of a multi-step task

---

## The `/compact` Command

Compaction summarizes your conversation while preserving key information:

```
> /compact
```

Or with instructions:

```
> /compact Focus on the API changes we discussed
```

**What gets preserved:**
- Important code patterns
- Key decisions made
- File states and modifications

**What gets summarized:**
- Exploratory conversation
- Failed attempts
- Verbose command outputs

---

## Auto-Compaction

Claude Code automatically compacts when you approach context limits. You'll see a message like:

```
Context compacted (was 95% full)
```

You can customize what survives compaction in your CLAUDE.md:
```
When compacting, always preserve the full list of modified files and test commands.
```

---

## Using Subagents to Preserve Context

One of the best ways to manage context is using **subagents**. When Claude explores your codebase, it reads many files - all consuming context.

Instead:
```
Use a subagent to investigate how authentication works in this codebase
```

The subagent works in its own context and returns only a summary!

---

## Practice: Context Comparison

Try this experiment:

### Round 1: Polluted Context
1. Ask Claude several unrelated questions
2. Make some changes, then undo them
3. Ask Claude to explain a function
4. Notice if Claude seems scattered

### Round 2: Clean Context
1. Run `/clear`
2. Directly ask Claude to explain the same function
3. Compare the quality of the response

---

## Best Practices

### Do:
- `/clear` between unrelated tasks
- Use subagents for exploration
- Keep prompts focused and specific
- Let Claude finish before course-correcting

### Don't:
- Keep correcting the same mistake repeatedly
- Ask many unrelated questions in one session
- Let context fill up without managing it

---

## Rewind: Undo Without Losing Everything

Made a mistake? Don't start over. Use **rewind**:

- Press `Esc` twice, or
- Type `/rewind`

This opens a checkpoint menu where you can:
- Restore conversation only (keep code changes)
- Restore code only (keep conversation)
- Restore both

---

## Key Takeaways

1. Context is your most precious resource
2. Use `/clear` between unrelated tasks
3. Use `/compact` when context gets high
4. Subagents keep your main context clean
5. A fresh start with a good prompt beats a polluted context

---

## Next Lesson

In Lesson 3, you'll learn about **CLAUDE.md** - how to give Claude persistent memory that survives across sessions.

Type `/lesson-03` to continue.
