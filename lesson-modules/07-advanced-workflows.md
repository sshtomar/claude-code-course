# Lesson 7: Advanced Workflows

This lesson covers advanced patterns: Plan Mode for safe analysis, parallel sessions with git worktrees, and headless mode for automation.

---

## Plan Mode

Plan Mode instructs Claude to analyze and plan without making changes. Perfect for:

- Exploring unfamiliar code safely
- Planning complex refactors
- Code review and analysis

### Entering Plan Mode

**During a session:** Press `Shift+Tab` twice (first goes to auto-accept, second to plan mode)

**Starting in plan mode:**
```bash
claude --permission-mode plan
```

**Headless query in plan mode:**
```bash
claude --permission-mode plan -p "analyze the auth system"
```

---

## Plan-First Workflow

The recommended approach for complex tasks:

### 1. Explore (Plan Mode)

```
> analyze the authentication module and explain how it works
```

Claude reads files but makes no changes.

### 2. Plan (Plan Mode)

```
> I want to add OAuth support. What files need to change?
```

Press `Ctrl+G` to open the plan in your editor for review.

### 3. Implement (Normal Mode)

Switch back with `Shift+Tab`, then:

```
> implement the OAuth flow from your plan
```

### 4. Verify and Commit

```
> run tests and commit if they pass
```

---

## Git Worktrees for Parallel Sessions

Run multiple Claude sessions with complete code isolation.

### What Are Worktrees?

Git worktrees let you check out multiple branches in separate directories, sharing the same git history.

### Creating Worktrees

```bash
# Create worktree with new branch
git worktree add ../project-feature-a -b feature-a

# Create worktree with existing branch
git worktree add ../project-bugfix bugfix-123
```

### Running Parallel Sessions

Terminal 1:
```bash
cd ../project-feature-a
claude
> implement the new dashboard
```

Terminal 2:
```bash
cd ../project-bugfix
claude
> fix the login bug
```

Each Claude session works on isolated code!

### Managing Worktrees

```bash
# List worktrees
git worktree list

# Remove when done
git worktree remove ../project-feature-a
```

---

## Headless Mode

Run Claude without an interactive session - perfect for scripts and CI/CD.

### One-Off Queries

```bash
claude -p "explain what this project does"
```

### Output Formats

```bash
# Plain text (default)
claude -p "summarize the README" --output-format text

# JSON (includes metadata)
claude -p "list API endpoints" --output-format json

# Streaming JSON
claude -p "analyze this code" --output-format stream-json
```

### Piping Data

```bash
# Pipe in
cat error.log | claude -p "explain this error"

# Pipe out
claude -p "generate a config file" > config.json

# Both
cat code.py | claude -p "add type hints" > typed_code.py
```

---

## Fan-Out Pattern

Process multiple files in parallel:

```bash
# Generate task list
claude -p "list all Python files needing migration"

# Process each
for file in $(cat files.txt); do
  claude -p "migrate $file to Python 3.12" \
    --allowedTools "Edit,Read"
done
```

---

## Writer/Reviewer Pattern

Use multiple sessions for quality:

### Session A (Writer)
```
> implement a rate limiter for the API
```

### Session B (Reviewer)
```
> review the rate limiter in src/middleware/rateLimiter.ts
> look for edge cases and race conditions
```

### Session A (Incorporate Feedback)
```
> here's the review feedback: [paste]. address these issues.
```

---

## Extended Thinking

For complex problems, Claude can use extended thinking - internal reasoning before responding.

### Enabling Thinking

- Toggle: `Option+T` (Mac) or `Alt+T` (Windows/Linux)
- Via config: `/config` → toggle thinking mode

### When to Use

- Complex architectural decisions
- Difficult debugging
- Multi-step planning
- Evaluating tradeoffs

### Viewing Thinking

Press `Ctrl+O` for verbose mode. Thinking appears as gray italic text.

---

## Session Management

### Naming Sessions

Give sessions memorable names:
```
> /rename oauth-migration
```

### Resuming

```bash
# Most recent in this directory
claude --continue

# Choose from recent
claude --resume

# By name
claude --resume oauth-migration
```

### From Inside Claude

```
> /resume
```

Opens a picker with keyboard navigation.

---

## Common Advanced Patterns

### Exploration with Subagents
```
> use subagents in parallel to research:
> 1. how auth works
> 2. how the database is structured
> 3. what APIs exist
```

### Adversarial Prompting
```
> implement the feature, then grill me on edge cases
> don't create a PR until I pass your test
```

### Iterative Refinement
```
> implement it simply first
> ... (Claude implements)
> now knowing what you know, scrap this and implement the elegant solution
```

---

## Debugging Sessions

### When Claude Seems Stuck

1. Check context usage - might be too full
2. Try `/clear` and start fresh with a better prompt
3. Break the task into smaller pieces

### When Claude Makes Repeated Mistakes

After 2 corrections on the same issue:
1. Run `/clear`
2. Write a new prompt incorporating what you learned
3. Fresh context + better prompt > polluted context

### When You Need to Undo

- `Esc` - Stop Claude mid-action
- `Esc Esc` - Open rewind menu
- `/rewind` - Same as double-Esc

---

## Practice Exercise

Try the Plan-First workflow:

1. Enter plan mode: `Shift+Tab` twice
2. Ask: "analyze this codebase and suggest 3 improvements"
3. Review Claude's analysis
4. Ask: "create a plan to implement improvement #1"
5. Press `Ctrl+G` to review the plan
6. Switch to normal mode: `Shift+Tab`
7. Say: "implement the plan"

---

## Key Takeaways

1. Plan Mode = safe exploration without changes
2. Git worktrees enable parallel Claude sessions
3. Headless mode integrates Claude into scripts
4. Writer/Reviewer pattern improves quality
5. Name and resume sessions for continuity

---

## Next Lesson

In Lesson 8, you'll learn **Best Practices** - patterns for getting consistently great results.

Type `/lesson-08` to continue.
