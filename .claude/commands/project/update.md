# /project:update - Update Project Progress

Update project progress in Linear and PROGRESS.md. Linear is the primary source of truth.

<instructions>
CRITICAL: Update Linear issues first, then PROGRESS.md as a journal!

## Linear-First Process

1. **Check Active Issues:**
   - Use `mcp__linear__list_my_issues` to see your assigned issues
   - Use `mcp__linear__list_issues` with project filter for current work
   - Identify which Linear issues relate to your completed work

2. **Update Linear Issues (PRIMARY):**
   - Use `mcp__linear__update_issue` to change status:
     - "Todo" → "In Progress" when starting
     - "In Progress" → "In Review" when complete
     - "In Review" → "Done" after merge
   - Add detailed comments with `mcp__linear__create_comment`:
     - Implementation approach taken
     - Design decisions and rationale
     - Challenges encountered and solutions
     - Links to commits
     - Files created/modified
     - Tests added
     - Performance considerations

3. **Update PROGRESS.md (JOURNAL):**
   - Read current PROGRESS.md
   - Add new entry as a development journal:
     - Date/time stamp
     - Summary of session's work
     - Linear issue references
     - Key decisions made
     - Lessons learned
   - This is a historical log, not primary documentation

4. **Update task-context.md:**
   - Clear completed tasks
   - Add any new working context
   - Keep minimal - just current session needs

## Journal Format (PROGRESS.md):
```markdown
## [Date] - [Session Summary]

### Completed
- [PRJ-XX] Implemented equipment data model
  - Created src/models/EquipmentModel.cs
  - Added 12 unit tests (all passing)
  - Design decision: Used value objects for combat modifiers

### In Progress
- [PRJ-YY] Working on service layer
  - Completed interface definition
  - Next: Implement business logic

### Notes
- Discovered issue with serialization - created [PRJ-ZZ] to track
- Performance consideration: May need caching for equipment lookups
```

## Best Practices:
- Linear comments = detailed technical documentation
- PROGRESS.md = development journal and narrative
- task-context.md = temporary working memory
- Always link Linear IDs in commits
- Document decisions in Linear, summarize in PROGRESS.md
</instructions>

<frequency>
- Update Linear immediately when changing task status
- Update Linear with detailed comments during work
- Update PROGRESS.md at session end as journal entry
- Clear task-context.md when switching features
</frequency>

<linear-commands>
- `mcp__linear__list_my_issues` - See your assigned issues
- `mcp__linear__list_issues` - Search all project issues
- `mcp__linear__update_issue` - Update issue status/details
- `mcp__linear__create_comment` - Add detailed progress notes
- `mcp__linear__get_issue` - Get full issue details
</linear-commands>