# /project:do-task - Execute Implementation Task

You are a senior engineer implementing a project plan for the task identified as $ARGUMENTS. This must be a Linear issue ID (e.g., PRJ-42).

**IMPORTANT:** Follow from Top to Bottom - don't skip anything and ultrathink.

**CREATE A TODO LIST** with exactly these 7 items:

1. Analyze task from Linear ID
2. Validate context and dependencies
3. Set status to in_progress
4. Execute task implementation
5. Execute code review
6. Complete task and commit changes
7. Generate final report

## 1 · Analyze task from Linear ID

- Parse the Linear issue ID from $ARGUMENTS (e.g., PRJ-42)
- If no ID provided, use `mcp__linear__list_my_issues` to show available tasks and ask user to specify
- Use `mcp__linear__get_issue` to retrieve full task details
- If anything is unclear in the task description, ask clarifying questions before continuing

## 2 · Validate context and dependencies

**CRITICAL CONTEXT VALIDATION:** Before executing any task, validate the context.

If task is small (≤1h effort or labeled 'small' in Linear), skip parallel validation and do a single combined check.

Otherwise, spin up Parallel Subagents (using Task tool) for these validations:

1. **Requirements Context:** Check parent requirements are understood
   - Get parent epic with `mcp__linear__get_issue`
   - Verify acceptance criteria are clear
   - Ensure requirements align with implementation

2. **Dependencies:** Check if dependent tasks need completion first
   - Use `mcp__linear__list_issues` to find related tasks
   - Check their status (must be "Done" or "In Review")
   - Identify any blockers

3. **Design Validation:** Ensure design decisions are documented
   - Get design issue from parent references
   - Verify architecture approach is clear
   - Check for any design constraints

4. **Sprint Alignment:** Confirm task fits current scope
   - Verify task is assigned to active sprint/cycle
   - Check it doesn't reference future functionality
   - Ensure it's properly prioritized

**IMPORTANT:** If validation fails or dependencies are unmet, pause and ask user for clarification.

## 3 · Set status to in_progress

- Use `date` command to get current timestamp (your idea of current datetime is probably wrong)
- Update Linear issue status to "In Progress" using `mcp__linear__update_issue`
- Add comment to Linear with `mcp__linear__create_comment`:
  ```
  Starting work on [Task Title]
  Session started: [YYYY-MM-DD HH:MM]
  Approach: [Brief summary of implementation plan]
  ```
- Create/update `task-context.md` with session information

## 4 · Execute task implementation

### Setup Working Context
Update `task-context.md` with:
```markdown
# Task Context - [Feature Name]

**Current Task:** [Linear ID] ([Task Title])
**Session Started:** [YYYY-MM-DD HH:MM]
**Parent Epic:** [Epic ID]
**Design Issue:** [Design ID]

## Working Notes
- [Implementation approach]
- [Key decisions]
```

This provides TDD-ready boilerplate with tests pre-configured.

### Follow TDD Process
**USE TEST DRIVEN DEVELOPMENT!** 
- If you run into this line, prompt the user that they may want to edit this file (do-task.md) with details of their testing environment, such as how to run a debug build. Give suggestions if you already have knowledge of how to do this.

Iterative TDD Process:

#### 1. RED Phase - Write failing test
- Write test that enforces new desired behavior
- Run test - it MUST fail
- Do NOT modify non-test code in this phase!
- Add progress comment to Linear: "RED: Added test for [behavior]"

#### 2. GREEN Phase - Make test pass  
- Write simplest code that makes test pass
- Do NOT modify tests in this phase!
- Test & commit to git (only when all tests pass), don't push
- Add progress comment to Linear: "GREEN: Implemented [feature]"

#### 3. REFACTOR Phase - Improve code
- Refactor for organization, readability, maintainability
- Follow Martin Fowler's refactoring guidance
- Minor refactors: single commit
- Major refactors: staged commits
- Test & commit (only when all tests pass)
- Add progress comment to Linear: "REFACTOR: [improvements made]"

**IMPORTANT:** If any implementation choice needs to be made, **ASK THE USER**:
- Present the choices clearly
- Argue pros and cons for each
- Wait for user decision

Repeat RED-GREEN-REFACTOR until feature complete.

### Progress Logging
After each TDD cycle:
- Add detailed comment to Linear issue
- Update `task-context.md` with progress
- Append to Linear comment log:
  ```
  [YYYY-MM-DD HH:MM]: Completed [what] - [brief detail]
  ```

## 5 · Execute Code Review

Follow these steps for Code Review (in order):

### Automated Checks
- Ensure all tests pass
- Check test coverage meets project standards

### Manual Review (use parallel subagents via Task tool)
Create parallel validation tasks for:
1. **Code Quality:** Follows project conventions, no magic numbers
2. **Error Handling:** Proper error cases covered
3. **Performance:** No obvious bottlenecks
4. **Security:** No exposed secrets, follows best practices

### Handle Review Results
- On **PASS**: 
  - Add comment to Linear: "Code review passed - all checks green"
  - Move to finalization
- On **FAIL**:
  - Thoroughly understand each issue
  - Add failed items as subtasks in TodoWrite
  - Fix each issue using TDD approach
  - Add comment to Linear documenting issues found and fixes
  - Re-run full review after fixes

## 6 · Complete Task

- Update Linear issue status to "In Review" using `mcp__linear__update_issue`
- Add completion comment to Linear:
  ```
  ✅ Task Implementation Complete
  
  📁 Files Modified:
  - src/file1.cs:123 (added X functionality)
  - test/file1Test.cs:45 (Y new tests)
  
  🧪 Tests: Z tests added, all passing
  Coverage: XX%
  
  🎯 Acceptance Criteria Met:
  - [x] Criterion 1 from requirements
  - [x] Criterion 2 from requirements
  
  📊 Performance: [Any relevant metrics]
  
  Status: Ready for review
  ```
- Commit all changes to git on main branch (but don't push yet)
- Clear completed task from `task-context.md`

## 7 · Handoff to Review

### Report to User
```
✅ Task Implementation Complete!

🔎 Task: [PRJ-XX] - [Task Title]  
📁 Changes: [N] files modified, [M] tests added
🧪 All tests passing

The task has been implemented following TDD practices and is ready for review.

Next step: Run /project:review [PRJ-XX] to:
- Perform comprehensive code review
- Run all quality checks
- Complete the task in Linear
- Get suggestion for next task

Or continue with another task: /project:do-task [PRJ-YY]
```

### Important Notes
- Task status is "In Review" not "Done" until review passes
- All changes are committed to main branch
- Review may require going back to implementation
- If review passes, status changes to "Done"

## Task Questions

The user may ask questions about tasks without wanting execution:
- "What's the next task?" - Check Linear for "Todo" tasks in the feature epic
- "What's the status?" - Show Linear issue statuses
- "What tasks are blocked?" - Check Linear for blocked issues
- "Show me the design" - Get the design issue from Linear

For questions, provide information without starting implementation.

## Context Management

The only local file is `task-context.md` which contains:
- Current task ID and title
- Key decisions made during implementation
- Temporary notes that don't belong in Linear yet
- Working code snippets

This file is working memory only - Linear has the permanent record.

## Linear MCP Commands Reference
- `mcp__linear__get_issue` - Get full task details
- `mcp__linear__update_issue` - Update task status
- `mcp__linear__create_comment` - Add progress notes
- `mcp__linear__list_issues` - Find related tasks
- `mcp__linear__list_my_issues` - See assigned tasks
- `mcp__linear__list_comments` - View task discussion

## Critical Rules
- Always create TodoWrite list at start
- Use parallel validation for context checking
- Follow strict TDD: RED-GREEN-REFACTOR
- Log all progress to Linear comments (not just task-context.md)
- Mandatory code review before pushing to main
- Provide structured final report and commit files to complete the full development cycle
