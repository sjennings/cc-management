# /project:linear-task

Create or update Linear tasks with full integration into the development workflow.

## Usage
```
/project:linear-task <action> [taskId] [options]
```

## Actions
- `create`: Create new task/epic
- `update`: Update existing task
- `start`: Begin work on task
- `complete`: Mark task as done
- `comment`: Add progress comment
- `list`: List tasks by criteria

## Options
- `--title`: Task title
- `--description`: Detailed description
- `--type`: epic|task|bug|improvement
- `--priority`: urgent|high|normal|low
- `--estimate`: Points estimate (1-8)
- `--parent`: Parent epic ID
- `--labels`: Comma-separated labels
- `--assignee`: Assign to user

## Workflow Integration

<workflow>
1. **Task Creation**
   - Validate Linear connection
   - Create with proper hierarchy
   - Add implementation details
   - Set initial status

2. **Starting Work**
   - Update status to "In Progress"
   - Create local task-context.md
   - Add initial comment
   - Note: All work happens on main branch

3. **Progress Updates**
   - Add technical comments
   - Update completion percentage
   - Link related commits
   - Document decisions

4. **Task Completion**
   - Update status to "Done"
   - Add completion summary
   - Link final commits
   - Clear local context
</workflow>

## Command Templates

<templates>
### Create New Feature Task
```typescript
// /project:linear-task create --type epic --title "Implement Save System"
const epic = await linear.createIssue({
  teamId: process.env.LINEAR_TEAM_ID,
  title: options.title,
  description: `## Feature: ${options.title}
  
### Overview
${await generateFeatureOverview(options.title)}

### Technical Approach
${await analyzeTechnicalRequirements(options.title)}

### Acceptance Criteria
- [ ] Data persistence working
- [ ] Save/load UI implemented
- [ ] Auto-save functionality
- [ ] Migration support
- [ ] Full test coverage

### Implementation Tasks
Will be created as sub-issues.`,
  estimate: options.estimate || 8,
  priority: options.priority || 2,
  labels: ["feature", ...options.labels]
});

// Create implementation tasks
const tasks = [
  { title: "Design save data schema", estimate: 2 },
  { title: "Implement serialization", estimate: 3 },
  { title: "Create save/load UI", estimate: 2 },
  { title: "Add auto-save system", estimate: 1 },
  { title: "Write integration tests", estimate: 2 }
];

for (const task of tasks) {
  await linear.createIssue({
    teamId: process.env.LINEAR_TEAM_ID,
    parentId: epic.id,
    ...task,
    description: await generateTaskDetails(task.title)
  });
}
```

### Update Task Progress
```typescript
// /project:linear-task update PRJ-42 --comment "Completed serialization"
const update = {
  stateId: await getStateId("In Progress"),
  comment: {
    body: `## Progress Update

### Completed
- ✅ Designed save data schema
- ✅ Implemented JSON serialization
- ✅ Added compression support

### Current Work
- 🔄 Creating save/load UI
  - File dialog integration
  - Progress indicators
  - Error handling

### Next Steps
- Auto-save timer implementation
- Settings persistence
- Cloud save preparation

### Technical Notes
- Using System.Text.Json for serialization
- Gzip compression reduces save size by ~70%
- Save files use .hexsave extension

### Code References
- SaveSystem.cs:45 - Main serialization logic
- SaveData.cs:12 - Data structure
- SaveLoadUI.cs:78 - UI implementation

Time spent: 3h 30m`
  }
};

await linear.updateIssue(options.taskId, update);
```

### Start Work Session
```typescript
// /project:linear-task start PRJ-42
const task = await linear.getIssue(options.taskId);

// Update status
await linear.updateIssue(task.id, {
  stateId: await getStateId("In Progress"),
  assigneeId: await getCurrentUserId()
});

// Add starting comment
await linear.createComment({
  issueId: task.id,
  body: `Starting work on: ${task.title}

### Session Goals
${await extractTaskGoals(task)}

### Approach
${await planImplementation(task)}

Working on: main branch
Session started: ${new Date().toISOString()}`
});

// Create local context
await createTaskContext({
  linearId: task.id,
  title: task.title,
  goals: await extractTaskGoals(task),
  parentEpic: task.parent?.id
});
```

### Complete Task
```typescript
// /project:linear-task complete PRJ-42
const task = await linear.getIssue(options.taskId);

// Gather completion info
const completionData = await gatherCompletionData();

// Update task
await linear.updateIssue(task.id, {
  stateId: await getStateId("Done"),
  comment: {
    body: `## Task Completed ✅

### Summary
${completionData.summary}

### Implementation Details
${completionData.technicalDetails}

### Test Coverage
- Unit Tests: ${completionData.unitTestCoverage}%
- Integration Tests: ${completionData.integrationTests}
- Performance: ${completionData.performanceMetrics}

### Files Changed
${completionData.filesChanged.map(f => `- ${f}`).join('\n')}

### Documentation Updated
- ✅ Code comments
- ✅ API documentation  
- ✅ ADR if applicable

### Commits: ${completionData.commitCount} commits to main

Time spent: ${completionData.timeSpent}`
  }
});

// Clear local context
await clearTaskContext(task.id);
```
</templates>

## Status Management

<status-management>
### Linear Workflow States
1. **Backlog** - Not yet planned
2. **Todo** - Ready to start
3. **In Progress** - Active work
4. **In Review** - Code review/QA
5. **Done** - Completed
6. **Canceled** - Won't implement

### Automatic Transitions
- Completing implementation → "In Review"
- Passing code review → "Done"
- Stale (>7 days) → Comment reminder
- Blocked → Add blocking label
</status-management>

## Integration Features

<integration>
### Git Integration
```bash
# Work directly on main branch
# No feature branches needed

# Commit with Linear reference
git commit -m "feat(save): Implement JSON serialization

Implements the core serialization logic for game saves using
System.Text.Json with custom converters.

Linear: PRJ-42"

# All commits reference Linear issue
# Work happens directly on main branch
```

### Code References
```csharp
// In code comments
// TODO(PRJ-42): Add compression support
// FIXME(PRJ-43): Handle circular references
// NOTE: See PRJ-44 for performance improvements
```

### Time Tracking
```yaml
# In Linear comments
Time spent: 2h 30m
Remaining estimate: 3h

# Automatic tracking via commits
[2h] Implemented serialization
[30m] Fixed unit tests
```
</integration>

## Best Practices

<best-practices>
1. **Task Granularity**
   - Tasks: 1-8 hours
   - Epics: 1-2 weeks
   - Break down larger work

2. **Description Quality**
   - Clear acceptance criteria
   - Technical approach
   - Test requirements
   - Dependencies noted

3. **Progress Updates**
   - Daily comments on active tasks
   - Technical details included
   - Blockers highlighted
   - Time tracking accurate

4. **Label Usage**
   - `blocked` - Waiting on dependency
   - `needs-review` - Ready for review
   - `technical-debt` - Refactoring
   - `breaking-change` - API changes

5. **Linking**
   - Reference related tasks
   - Link to commits on main
   - Connect to documentation
   - Cross-reference bugs
</best-practices>

## Query Examples

<queries>
### List My Active Tasks
```
/project:linear-task list --assignee me --status "In Progress"
```

### Find Tasks by Label
```
/project:linear-task list --labels "save-system,high-priority"
```

### Show Epic Progress
```
/project:linear-task list --parent HEX-40 --include-completed
```

### Search by Text
```
/project:linear-task list --search "serialization"
```
</queries>

## Error Handling

<error-handling>
### Common Issues
1. **API Rate Limits**
   - Cache responses
   - Batch operations
   - Retry with backoff

2. **Invalid State Transitions**
   - Check current state first
   - Use valid workflow states
   - Handle edge cases

3. **Missing References**
   - Validate IDs exist
   - Handle deleted items
   - Provide clear errors

4. **Network Failures**
   - Queue updates locally
   - Retry mechanism
   - Offline mode support
</error-handling>

## Example Usage

```bash
# Create new feature epic
/project:linear-task create --type epic --title "Save System" --estimate 8

# Start work on task
/project:linear-task start PRJ-42

# Add progress comment
/project:linear-task comment PRJ-42 --comment "Completed serialization, starting on UI"

# Complete task
/project:linear-task complete PRJ-42

# List my tasks
/project:linear-task list --assignee me --status "Todo,In Progress"
```