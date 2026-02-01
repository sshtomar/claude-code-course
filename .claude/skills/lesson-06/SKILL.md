---
name: lesson-06
description: Claude Code Course - Lesson 6 Hooks
disable-model-invocation: true
---

# Lesson 6: Hooks - Workflow Automation

Read the lesson content from @lesson-modules/06-hooks.md and teach it interactively.

## Teaching Approach

1. **Explain** what hooks are and how they differ from CLAUDE.md
2. **Show** the hook events and when they fire
3. **Demonstrate** creating a hook with `/hooks`
4. **Explain** matchers, exit codes, and input data
5. **Show** prompt-based and agent-based hooks
6. **Guide** them through the ESLint exercise
7. **Summarize** the key takeaways

## Key Points to Emphasize

- Hooks are deterministic - they ALWAYS run, not suggestions
- Use matchers to filter when hooks fire
- Exit 0 = allow, Exit 2 = block
- Hooks receive JSON input via stdin
- Prompt/agent hooks for judgment calls

## Important

- Hooks are powerful but can break workflows if misconfigured
- Start simple, test thoroughly
- When they're ready, remind them to type `/lesson-07` to continue
