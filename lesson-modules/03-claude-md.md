# Lesson 3: CLAUDE.md - Persistent Memory

CLAUDE.md is a special file that Claude reads at the start of every conversation. It's your way to give Claude project-specific knowledge that persists across sessions.

---

## What Goes in CLAUDE.md?

Include things Claude **can't infer from code alone**:

| Include | Don't Include |
|---------|---------------|
| Build commands | Standard language conventions |
| Code style rules that differ from defaults | Obvious patterns in your code |
| Testing instructions | Detailed API docs (link instead) |
| Architectural decisions | File-by-file descriptions |
| Common gotchas | Information that changes often |

---

## Creating Your First CLAUDE.md

The easiest way to start:

```
> /init
```

This analyzes your codebase and generates a starter CLAUDE.md. You can then refine it.

Or create one manually at your project root:

```
> create a CLAUDE.md file for this project
```

---

## CLAUDE.md Locations

| Location | Applies To | Shared With |
|----------|------------|-------------|
| `~/.claude/CLAUDE.md` | All your projects | Just you |
| `./CLAUDE.md` | This project | Team (via git) |
| `./.claude/CLAUDE.md` | This project | Team (via git) |
| `./CLAUDE.local.md` | This project | Just you (gitignored) |

Files higher in the hierarchy load first, with lower files adding to them.

---

## Example CLAUDE.md

```markdown
# Project: My Web App

## Build Commands
- `npm run dev` - Start development server
- `npm test` - Run tests
- `npm run lint` - Run linter

## Code Style
- Use TypeScript strict mode
- Prefer functional components
- Use named exports, not default exports

## Architecture
- `/src/components` - React components
- `/src/hooks` - Custom hooks
- `/src/api` - API client functions

## Important Notes
- Always run tests before committing
- The auth module uses JWT tokens stored in httpOnly cookies
- Never commit .env files
```

---

## Writing Effective Instructions

### Be Specific, Not Vague

| Vague | Specific |
|-------|----------|
| "Format code properly" | "Use 2-space indentation" |
| "Write good tests" | "Use Jest with React Testing Library" |
| "Handle errors well" | "Use try/catch and log to Sentry" |

### Use Structure

```markdown
## Testing
- Run single tests with `npm test -- --grep "test name"`
- Mock external APIs using `msw`
- Coverage must stay above 80%
```

---

## Importing Other Files

CLAUDE.md can reference other files with `@`:

```markdown
See @README.md for project overview.
See @package.json for available scripts.

## Guidelines
- Follow conventions in @docs/style-guide.md
- Personal settings: @~/.claude/my-preferences.md
```

This keeps CLAUDE.md focused while letting Claude access detailed docs when needed.

---

## Modular Rules with .claude/rules/

For larger projects, organize rules into separate files:

```
.claude/
├── CLAUDE.md           # Main instructions
└── rules/
    ├── code-style.md   # Style guidelines
    ├── testing.md      # Testing conventions
    └── security.md     # Security requirements
```

All `.md` files in `.claude/rules/` are automatically loaded.

---

## Path-Specific Rules

Rules can apply only to certain files:

```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API Development Rules

- All endpoints must include input validation
- Use the standard error response format
- Include OpenAPI documentation comments
```

This rule only loads when Claude works with files matching the pattern.

---

## Common CLAUDE.md Mistakes

### Too Long
If Claude ignores your instructions, the file is probably too long. Keep it under 200 lines.

### Obvious Information
Don't include things Claude already knows:
```markdown
# Bad - Claude knows JavaScript
- Use const for constants
- Use let for variables
```

### Duplicate of README
CLAUDE.md is for **instructions**, not documentation. Link to docs instead.

---

## Verifying CLAUDE.md Works

After creating CLAUDE.md, test it:

```
> what are the build commands for this project?
```

Claude should answer using your CLAUDE.md content.

Run `/memory` to see all loaded memory files and their contents.

---

## Practice Exercise

1. Run `/init` to generate a starter CLAUDE.md
2. Review and customize it
3. Add one project-specific rule
4. Test by asking Claude about that rule
5. Run `/memory` to see what's loaded

---

## Key Takeaways

1. CLAUDE.md gives Claude persistent project knowledge
2. Keep it concise - only include what Claude can't infer
3. Use `@` imports to reference detailed docs
4. Organize large projects with `.claude/rules/`
5. Test your rules to make sure they work

---

## Next Lesson

In Lesson 4, you'll learn about **Skills** - reusable instructions and workflows you can invoke with slash commands.

Type `/lesson-04` to continue.
