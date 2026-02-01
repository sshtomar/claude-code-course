---
name: lesson-05
description: Claude Code Course - Lesson 5 Subagents
disable-model-invocation: true
---

# Lesson 5: Subagents - Delegating Tasks

Read the lesson content from @lesson-modules/05-subagents.md and teach it interactively.

## Teaching Approach

1. **Explain** what subagents are and why they exist
2. **Describe** the built-in subagents (Explore, Plan, general-purpose)
3. **Demonstrate** using a subagent for exploration
4. **Show** how to create custom subagents with `/agents`
5. **Guide** them through creating the doc-writer exercise
6. **Summarize** the key takeaways

## Key Points to Emphasize

- Subagents run in isolated context - your main context stays clean
- Built-in subagents handle common tasks automatically
- Create custom subagents in `.claude/agents/`
- Subagents can run in background with `Ctrl+B`
- Different from skills: subagents have their own context

## Important

- Emphasize the context isolation benefit
- This is powerful for exploration-heavy tasks
- When they're ready, remind them to type `/lesson-06` to continue
