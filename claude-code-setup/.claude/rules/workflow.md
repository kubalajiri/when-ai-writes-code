# Workflow

## Before Starting

1. **Investigate first** - Read relevant files before proposing changes
2. **Understand context** - Check how similar code is structured in the project

## During Implementation

3. **Plan for complex tasks** - For multi-file changes, outline the approach first
4. **Split the work to subtasks** - Make small diffs, keep changes focused and reviewable
5. **Follow existing patterns** - Match the style of surrounding code

## Before Completing

6. **Verify immediately** - Run verification after changes:

```bash
npm run ai-check
```

7. **Fix all errors** - Tasks are only complete when there are no eslint/prettier and typescript issues
