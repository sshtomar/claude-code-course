# Lesson 4: Skills - Custom Slash Commands

Skills extend Claude's capabilities with reusable instructions and workflows. Create a skill once, invoke it anytime with `/skill-name`.

---

## What Are Skills?

Skills are markdown files that teach Claude how to do specific things:

- **Reference skills**: Domain knowledge, coding conventions, style guides
- **Task skills**: Step-by-step workflows like deploying or creating PRs

When you type `/skill-name`, Claude loads that skill's instructions.

---

## Skill Locations

| Location | Available To |
|----------|--------------|
| `~/.claude/skills/<name>/SKILL.md` | All your projects |
| `.claude/skills/<name>/SKILL.md` | This project only |

---

## Creating Your First Skill

Let's create a simple code review skill.

### Step 1: Create the Directory

```bash
mkdir -p .claude/skills/review
```

### Step 2: Create SKILL.md

```markdown
---
name: review
description: Review code for quality and best practices
---

When reviewing code:

1. Check for bugs and edge cases
2. Evaluate code clarity and naming
3. Look for security issues
4. Suggest performance improvements

Format your review as:
- **Critical**: Must fix before merging
- **Warning**: Should fix
- **Suggestion**: Nice to have
```

### Step 3: Use It

```
> /review src/auth.ts
```

---

## Skill Frontmatter

The YAML section between `---` markers configures the skill:

```yaml
---
name: deploy
description: Deploy to production
disable-model-invocation: true
allowed-tools: Bash, Read
model: sonnet
---
```

| Field | Purpose |
|-------|---------|
| `name` | The `/slash-command` name |
| `description` | Helps Claude know when to use it |
| `disable-model-invocation` | Only you can invoke (not Claude) |
| `allowed-tools` | Limit what tools the skill can use |
| `model` | Which model to use |

---

## Two Types of Skills

### Reference Skills (Claude Can Invoke)

For knowledge Claude should apply automatically:

```yaml
---
name: api-style
description: API design conventions for our services
---

When writing API endpoints:
- Use kebab-case for URLs
- Use camelCase for JSON properties
- Always include pagination
```

Claude loads this when working on APIs without you asking.

### Task Skills (You Invoke)

For workflows with side effects:

```yaml
---
name: deploy
description: Deploy to production
disable-model-invocation: true
---

Deploy the application:
1. Run the test suite
2. Build the application
3. Push to production
4. Verify deployment
```

`disable-model-invocation: true` prevents Claude from deploying on its own!

---

## Using Arguments

Skills can accept arguments with `$ARGUMENTS`:

```yaml
---
name: fix-issue
description: Fix a GitHub issue
disable-model-invocation: true
---

Fix GitHub issue $ARGUMENTS:

1. Read the issue with `gh issue view $ARGUMENTS`
2. Understand the problem
3. Implement the fix
4. Write tests
5. Commit with message "fix: closes #$ARGUMENTS"
```

Usage:
```
> /fix-issue 123
```

### Multiple Arguments

Access specific arguments by position:

```yaml
---
name: migrate
description: Migrate component between frameworks
---

Migrate $0 component from $1 to $2.
```

Usage:
```
> /migrate SearchBar React Vue
```

---

## Adding Supporting Files

Skills can include additional files:

```
my-skill/
├── SKILL.md           # Main instructions
├── template.md        # Template Claude fills in
├── examples/
│   └── sample.md      # Example outputs
└── scripts/
    └── helper.sh      # Script Claude can run
```

Reference them from SKILL.md:
```markdown
See [template.md](template.md) for the output format.
See [examples/sample.md](examples/sample.md) for an example.
```

---

## Dynamic Context with Shell Commands

Inject live data into skills with `` !`command` ``:

```yaml
---
name: pr-summary
description: Summarize the current PR
---

## Current Changes
!`git diff main`

## Recent Commits
!`git log --oneline -5`

Summarize these changes for a PR description.
```

The commands run before Claude sees the skill, so Claude gets actual data.

---

## Running Skills in Subagents

For skills that should run in isolation:

```yaml
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

Research $ARGUMENTS thoroughly:
1. Find relevant files
2. Analyze the code
3. Summarize findings
```

`context: fork` runs in a separate context, keeping your main conversation clean.

---

## Built-in Skills

Claude Code comes with useful skills. Type `/` to see what's available:

- `/init` - Initialize CLAUDE.md
- `/memory` - View memory files
- `/permissions` - Manage permissions
- `/compact` - Compact context

---

## Practice Exercise

Create a commit skill:

1. Create `.claude/skills/commit/SKILL.md`:

```yaml
---
name: commit
description: Create a well-formatted commit
disable-model-invocation: true
---

Create a git commit:
1. Run `git status` to see changes
2. Run `git diff --staged` to review staged changes
3. Write a commit message following conventional commits
4. Format: type(scope): description
5. Commit the changes
```

2. Stage some changes
3. Run `/commit`

---

## Key Takeaways

1. Skills are reusable instructions in SKILL.md files
2. Use `disable-model-invocation: true` for dangerous operations
3. Pass data with `$ARGUMENTS`
4. Inject dynamic content with `` !`command` ``
5. Use `context: fork` to run in isolation

---

## Next Lesson

In Lesson 5, you'll learn about **Subagents** - specialized AI assistants for delegating tasks.

Type `/lesson-05` to continue.
