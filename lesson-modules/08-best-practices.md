# Lesson 8: Best Practices

This final lesson covers the patterns and practices that lead to consistently great results with Claude Code.

---

## The Golden Rule

**Give Claude a way to verify its work.**

This is the single highest-leverage thing you can do.

| Without Verification | With Verification |
|---------------------|-------------------|
| "implement email validation" | "implement email validation. test: user@example.com → true, invalid → false. run the tests" |
| "fix the bug" | "fix the bug and verify by running `npm test`" |
| "update the UI" | "update the UI, take a screenshot, compare to the mockup" |

---

## Be Specific

Vague prompts lead to multiple rounds of correction. Specific prompts get it right the first time.

### Instead of...

| Vague | Specific |
|-------|----------|
| "fix the bug" | "fix the login bug where users see a blank screen after wrong credentials" |
| "add tests" | "add tests for the edge case where user is logged out, using Jest" |
| "make it faster" | "optimize the database query in getUserOrders, it's doing N+1 queries" |

### Include Context

```
add input validation to the registration form.
check: email format, password 8+ chars, username alphanumeric only.
show inline errors. use the existing ErrorMessage component.
```

---

## Explore First, Then Implement

Don't let Claude jump straight to coding for unfamiliar codebases.

### Bad Pattern
```
> add OAuth support
(Claude guesses at the architecture)
(You correct it 5 times)
(Context is polluted)
```

### Good Pattern
```
> use a subagent to analyze how authentication currently works
(Subagent explores, returns summary)
> based on that, create a plan for adding OAuth
(Claude plans with understanding)
> implement the plan
```

---

## Manage Context Aggressively

### Clear Between Tasks
```
> /clear
```
Use liberally between unrelated tasks.

### Use Subagents for Exploration
```
> use a subagent to find all files related to payments
```
Keeps exploration out of your context.

### Compact When Needed
```
> /compact focus on the API changes
```
When context is high but you need continuity.

---

## Course-Correct Early

The best results come from tight feedback loops.

| Action | When |
|--------|------|
| `Esc` | Claude is going wrong direction |
| `Esc Esc` | Need to rewind to earlier state |
| "stop, let's try a different approach" | Mid-task redirection |
| `/clear` + better prompt | After 2+ failed corrections |

**Key insight:** A fresh start with a good prompt beats a long session with accumulated corrections.

---

## Write Effective CLAUDE.md

### Do Include

- Build/test commands Claude can't guess
- Code style rules that differ from defaults
- Important architectural decisions
- Common gotchas

### Don't Include

- Standard conventions Claude knows
- Obvious patterns in your code
- Long documentation (link instead)
- Information that changes frequently

### Keep It Short

If Claude ignores your rules, the file is probably too long. Aim for under 200 lines.

---

## Use the Right Tool

| Task | Tool |
|------|------|
| Project-wide instructions | CLAUDE.md |
| Reusable workflows | Skills |
| Isolated exploration | Subagents |
| Guaranteed actions | Hooks |
| Safe analysis | Plan Mode |

---

## Effective Prompting Patterns

### Step-by-Step Instructions
```
1. create a database migration for user profiles
2. create an API endpoint for getting/updating profiles
3. build a settings page for editing profiles
4. add tests for the API endpoint
```

### Let Claude Explore First
```
before making changes, analyze the current implementation
```

### Adversarial Review
```
implement this, then review it critically
find edge cases I haven't considered
```

### Elegant Refactoring
```
implement it simply first
...
now that you understand the problem, implement the elegant solution
```

---

## Common Failure Patterns

### The Kitchen Sink Session

**Problem:** You ask unrelated questions in one session. Context is cluttered.

**Fix:** `/clear` between unrelated tasks.

### Repeated Corrections

**Problem:** You correct Claude repeatedly on the same issue. Context is polluted with failed attempts.

**Fix:** After 2 corrections, `/clear` and write a better initial prompt.

### Over-Specified CLAUDE.md

**Problem:** CLAUDE.md is too long. Claude ignores half of it.

**Fix:** Ruthlessly prune. If Claude does something correctly without the instruction, remove it.

### The Trust Gap

**Problem:** Claude produces plausible-looking code that fails on edge cases.

**Fix:** Always provide verification (tests, commands, screenshots).

### Infinite Exploration

**Problem:** You ask Claude to "investigate" without scope. It reads hundreds of files.

**Fix:** Scope investigations narrowly or use subagents.

---

## Workflow Checklist

Before starting a task:

- [ ] Is my context clean? (`/clear` if needed)
- [ ] Do I have a way to verify success?
- [ ] Is my prompt specific enough?
- [ ] Should I explore first? (use subagent or plan mode)

During the task:

- [ ] Is Claude on the right track?
- [ ] Should I course-correct? (`Esc` or redirect)
- [ ] Is context getting full? (`/compact` if needed)

After the task:

- [ ] Did I verify the result?
- [ ] Should I add anything to CLAUDE.md?
- [ ] Should I create a skill for this workflow?

---

## Quick Reference

| Shortcut | Action |
|----------|--------|
| `Esc` | Stop Claude |
| `Esc Esc` | Rewind menu |
| `Shift+Tab` | Cycle permission modes |
| `Ctrl+G` | Open plan in editor |
| `Ctrl+O` | Toggle verbose mode |
| `Ctrl+B` | Background current task |
| `Option+T` | Toggle thinking mode |

| Command | Purpose |
|---------|---------|
| `/clear` | Reset context |
| `/compact` | Summarize context |
| `/memory` | View loaded memory files |
| `/agents` | Manage subagents |
| `/hooks` | Manage hooks |
| `/rewind` | Restore previous state |

---

## Key Takeaways

1. **Verify**: Always give Claude a way to verify its work
2. **Be specific**: Vague prompts = multiple corrections
3. **Explore first**: Don't let Claude guess at architecture
4. **Manage context**: Clear between tasks, use subagents
5. **Course-correct early**: Fresh start beats polluted context
6. **Keep CLAUDE.md short**: Long = ignored
7. **Use the right tool**: Skills, subagents, hooks each have their place

---

## Congratulations!

You've completed the Claude Code course! 🎉

You now understand:
- Basic usage and navigation
- Context management
- CLAUDE.md configuration
- Skills and custom commands
- Subagents for delegation
- Hooks for automation
- Advanced workflows
- Best practices

### What's Next?

1. Practice these patterns in your daily work
2. Create skills for your common workflows
3. Build custom subagents for your domain
4. Contribute to your team's CLAUDE.md
5. Explore the [official documentation](https://code.claude.com/docs)

---

Type `/course` to see the full course outline again.
