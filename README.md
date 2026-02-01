# Claude Code Interactive Course

An interactive course for learning Claude Code, designed to be taken inside a Claude Code terminal session.

## Inspiration

This course was inspired by [Boris Cherny's Twitter thread](https://x.com/bcherny/status/2017742741636321619) sharing productivity tips from the Claude Code team at Anthropic. Boris is the creator of Claude Code, and his thread covers best practices like using git worktrees for parallel sessions, the plan-first workflow, and effective use of CLAUDE.md files.

## Quick Start

1. Navigate to this directory:
   ```bash
   cd claude-code-course
   ```

2. Start Claude Code:
   ```bash
   claude
   ```

3. View the course overview:
   ```
   /course
   ```

4. Start the first lesson:
   ```
   /lesson-01
   ```

## Course Structure

```
claude-code-course/
├── README.md                    # This file
├── lesson-modules/              # Lesson content
│   ├── 01-getting-started.md
│   ├── 02-context-management.md
│   ├── 03-claude-md.md
│   ├── 04-skills.md
│   ├── 05-subagents.md
│   ├── 06-hooks.md
│   ├── 07-advanced-workflows.md
│   └── 08-best-practices.md
└── .claude/                     # Claude Code configuration
    ├── CLAUDE.md                # Course context
    └── skills/                  # Slash commands
        ├── course/SKILL.md      # /course - overview
        ├── lesson-01/SKILL.md   # /lesson-01
        ├── lesson-02/SKILL.md   # /lesson-02
        ├── lesson-03/SKILL.md   # /lesson-03
        ├── lesson-04/SKILL.md   # /lesson-04
        ├── lesson-05/SKILL.md   # /lesson-05
        ├── lesson-06/SKILL.md   # /lesson-06
        ├── lesson-07/SKILL.md   # /lesson-07
        └── lesson-08/SKILL.md   # /lesson-08
```

## Lessons

| # | Title | Topics | Command |
|---|-------|--------|---------|
| 1 | Getting Started | Installation, basic usage, permissions | `/lesson-01` |
| 2 | Context Management | Context window, /clear, /compact | `/lesson-02` |
| 3 | CLAUDE.md | Persistent memory, project config | `/lesson-03` |
| 4 | Skills | Custom slash commands, workflows | `/lesson-04` |
| 5 | Subagents | Task delegation, context isolation | `/lesson-05` |
| 6 | Hooks | Automation, pre/post tool hooks | `/lesson-06` |
| 7 | Advanced Workflows | Plan mode, worktrees, headless | `/lesson-07` |
| 8 | Best Practices | Patterns for great results | `/lesson-08` |

## How It Works

1. **Lesson content** lives in `lesson-modules/` as standalone markdown files
2. **Slash commands** in `.claude/skills/` reference the lesson files
3. When you type `/lesson-XX`, Claude reads the lesson and teaches it interactively
4. Each lesson includes practice exercises to try in real-time

## Features

- **Interactive**: Claude teaches each lesson conversationally
- **Hands-on**: Practice exercises in every lesson
- **Self-paced**: Jump to any lesson at any time
- **Real environment**: Learn by doing in an actual Claude Code session

## Customizing

### Add a New Lesson

1. Create `lesson-modules/09-new-topic.md` with the content
2. Create `.claude/skills/lesson-09/SKILL.md`:
   ```yaml
   ---
   name: lesson-09
   description: Claude Code Course - Lesson 9 New Topic
   disable-model-invocation: true
   ---

   # Lesson 9: New Topic

   Read the lesson content from @lesson-modules/09-new-topic.md
   and teach it interactively.
   ```
3. Update the `/course` skill to include the new lesson

### Modify Teaching Style

Edit `.claude/CLAUDE.md` to adjust how Claude teaches the material.

## Requirements

- Claude Code installed (`curl -fsSL https://claude.ai/install.sh | bash`)
- Active Claude subscription (Pro, Max, Teams, or Enterprise)

## Credits

- **Inspiration**: [Boris Cherny's Twitter thread](https://x.com/bcherny/status/2017742741636321619) on Claude Code productivity tips
- **Documentation**: [Official Claude Code docs](https://code.claude.com/docs)
- **Creator**: Boris Cherny ([@bcherny](https://x.com/bcherny)) created Claude Code at Anthropic

## License

Educational use. Based on official Claude Code documentation and community best practices.
