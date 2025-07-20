# Task Context Template

This template shows the structure for `task-context.md`, the only local file maintained during feature development. Copy this to the project root when starting new work.

```markdown
# Task Context - [Feature Name]

**Feature Epic:** [Linear Epic ID]
**Current Task:** [Linear Task ID] ([Task Title])
**Session Started:** [YYYY-MM-DD HH:MM]

## Working Notes
- [Implementation decisions made this session]
- [Technical challenges encountered]
- [Solutions being explored]
- [Performance considerations]

## Key Links
- Design Issue: [PRJ-XX]
- Requirements: [PRJ-YY, PRJ-ZZ]
- Related ADR: [ADR-XXX]

## Next Steps
- [What to do next in current task]
- [Next task to pick up]
- [Any blockers to resolve]

## Code Snippets
```csharp
// Temporary code snippets for reference
// Delete when committed
```

---
⚠️ This file is temporary working memory only.
- Clear when switching features
- Don't commit to version control
- Linear has the permanent record
```

## Usage Guidelines

### When to Create
- Starting work on a new feature
- Switching between features
- Beginning a new development session

### What to Include
- Only information needed for the current session
- Quick references to Linear issues
- Temporary code snippets being worked on
- Implementation decisions not yet ready for Linear

### What NOT to Include
- Permanent documentation (goes in Linear)
- Completed task information (goes in Linear)
- Historical decisions (goes in Linear)
- Team communication (goes in Linear)

### When to Clear
- Feature work is complete
- Switching to different feature
- All notes have been added to Linear
- End of work week

### Example Lifecycle

1. **Start Feature**
   ```bash
   cp .claude/commands/project/task-context-template.md task-context.md
   # Edit with feature details
   ```

2. **During Work**
   - Add implementation notes
   - Track current progress
   - Note decisions for Linear

3. **End Session**
   - Copy important notes to Linear comments
   - Update task status in Linear
   - Keep only what's needed for next session

4. **Complete Feature**
   ```bash
   rm task-context.md
   # All important info is now in Linear
   ```

This approach keeps local files minimal while Linear remains the authoritative source for all project information.