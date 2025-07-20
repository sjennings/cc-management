# Linear-First Project Management

This document explains how this project uses Linear as the exclusive source of truth for project management, with minimal local files for immediate working context.

## Philosophy

Linear is the canonical knowledge store for all project information:
- Requirements, designs, and tasks live exclusively in Linear
- Local files are temporary working memory only
- PROGRESS.md serves as a development journal, not primary documentation
- `task-context.md` holds immediate session context only

## Complete Development Workflow

The master workflow follows this progression:
1. **Feature Request** → `/project:init-feature` (creates Linear epic with requirements)
2. **Sprint Planning** → `/project:sprint-planning` (breaks down into tasks)
3. **Task Execution** → `/project:do-task` (TDD implementation)
4. **Code Review** → `/project:review` (automated quality checks)
5. **Next Task** → Repeat from step 3

## Project Management Commands

### Core Workflow Commands

#### 1. `/project:init-feature` - Feature Initialization
- Creates parent epic in Linear with EARS-formatted requirements
- Each requirement becomes a Linear sub-issue with acceptance criteria
- Optionally scaffolds code patterns (systems, components, resources)
- NO local requirements.md files - Linear is the source of truth
- Updates `task-context.md` with epic ID for current session only

#### 2. `/project:sprint-planning` - Sprint Planning from Milestones
- Breaks milestones into 2-week sprint cycles in Linear
- Interactive epic grooming with clarifying questions
- Creates technical design documents as Linear sub-issues
- Generates TDD-ready implementation tasks with estimates
- Assigns tasks to appropriate sprint cycles
- NO local sprint folders - Linear cycles are the source of truth

#### 3. `/project:do-task` - Task Execution
- Works exclusively with Linear issue IDs (e.g., PRJ-42)
- Validates context and dependencies from Linear hierarchy
- Updates Linear status: Backlog → In Progress → In Review
- Follows RED → GREEN → REFACTOR TDD cycle
- Uses `task-context.md` for temporary implementation notes
- Commits locally (doesn't push unless requested)

#### 4. `/project:review` - Code Review
- Reviews completed work at task/sprint/milestone levels
- Most commonly triggered automatically by do-task completion
- Runs automated tests and quality checks
- Creates comprehensive review reports in Linear comments
- Updates task status to "Done" after successful review

#### 5. `/project:update` - Progress Updates  
- Updates Linear issues as primary documentation
- PROGRESS.md maintained as development journal
- Linear comments contain detailed technical documentation
- `task-context.md` cleared between feature work

### Support Commands

#### 6. `/project:debug` - Bug Investigation
- Systematic bug investigation using Five Whys methodology
- Creates reproduction tests first (TDD approach)
- Documents full investigation process in Linear
- Creates bug issues with root cause analysis

#### 7. `/project:linear-task` - Direct Linear Operations
- Low-level Linear task management
- Create, update, start, complete tasks
- Add comments and attachments
- Query and list tasks by various criteria
- Template-based issue creation

### Documentation Templates

#### 8. `/project:task-context-template` - Working Memory Template
- Template for temporary session notes
- Cleared between features
- Only permanent record stays in Linear

## Linear Project Structure

```
Your Project Development (Project)
├── Milestone Epics (PRJ-5 through PRJ-13)
│   ├── Feature Epics
│   │   ├── Requirement Issues
│   │   ├── Implementation Tasks
│   │   └── Bug Reports
│   └── ADR Issues
└── Standalone Issues (bugs, improvements)
```

## Key Linear MCP Commands

### Project Management
- `mcp__linear__list_projects` - View all projects
- `mcp__linear__list_teams` - View teams
- `mcp__linear__get_project` - Get project details

### Issue Management
- `mcp__linear__create_issue` - Create new issues/epics
- `mcp__linear__update_issue` - Update status, details
- `mcp__linear__list_issues` - Search and filter issues
- `mcp__linear__get_issue` - Get full issue details

### Collaboration
- `mcp__linear__create_comment` - Add comments
- `mcp__linear__list_comments` - View discussion
- `mcp__linear__list_my_issues` - See assigned work

### Organization
- `mcp__linear__list_issue_labels` - Available labels
- `mcp__linear__list_issue_statuses` - Workflow states
- `mcp__linear__list_cycles` - Sprint cycles

## Best Practices

### 1. Issue Hierarchy
- **Milestones**: Top-level epics (e.g., "Milestone 7: Complete Editor Suite")
- **Features**: Child epics under milestones
- **Tasks**: Individual work items under features
- **ADRs**: Standalone issues linked to relevant features

### 2. Status Management
- **Backlog**: Not yet planned
- **Todo**: Ready to work on
- **In Progress**: Active development
- **In Review**: Code review/testing
- **Done**: Completed and merged

### 3. Labels
- `requirement`: For requirement issues
- `adr`: For architecture decisions  
- `bug`: For defect tracking
- `editor`: For editor-related work
- `milestone-X`: For milestone tracking

### 4. Synchronization
- Always update both Linear and local docs
- Reference Linear IDs in commits and PRs
- Use Linear comments for design discussions
- Keep PROGRESS.md as the detailed journal

## Migration Notes

For existing work:
1. Current milestones (1-6) remain in local docs only
2. New work (Milestone 7+) uses Linear tracking
3. PROGRESS.md continues as the source of truth for detailed history
4. Linear provides real-time status and collaboration

## Benefits

1. **Visibility**: Team-wide view of progress
2. **Collaboration**: Threaded discussions on issues  
3. **Integration**: Links between related work
4. **Metrics**: Velocity and progress tracking
5. **Search**: Powerful filtering and search
6. **Mobile**: Access from anywhere

## The Only Local File: task-context.md

```markdown
# Task Context - Equipment Editor

**Feature Epic:** PRJ-14
**Current Task:** PRJ-17 (Equipment Model Implementation)
**Session Started:** 2025-01-16 14:30

## Working Notes
- Decided to use value objects for combat modifiers
- Need to handle null equipment gracefully
- Consider caching for performance

## Next Steps
- Complete serialization tests
- Move to PRJ-18 (Service Layer)

---
This file is temporary. Clear when switching features.
```

## Workflow Example

```bash
# 0. Plan sprints for milestone
/project:sprint-planning PRJ-7

# Plans Sprint 7.1, 7.2, etc. with:
# - Epic breakdown and grooming
# - Task estimates and dependencies  
# - Sprint capacity planning

# 1. Start new feature
/project:init-feature Equipment Editor

# Creates in Linear:
# - Epic: "Equipment Editor" with EARS requirements
# - Requirement sub-issues with acceptance criteria
# Optionally scaffolds:
# - Code patterns (systems, components, resources)
# Updates locally:
# - task-context.md with epic ID

# 2. Execute a task
/project:do-task PRJ-17

# Validates:
# - Task context and dependencies from Linear
# Updates in Linear:
# - Status: Backlog → In Progress
# - Adds implementation progress comments
# Follows TDD:
# - RED → GREEN → REFACTOR cycle
# Updates locally:
# - task-context.md with session notes

# 3. Review completed work
/project:review PRJ-17

# Runs:
# - Automated tests and quality checks
# - Code review analysis
# Updates in Linear:
# - Status: In Review → Done
# - Adds review report as comment

# 4. Track progress
/project:update

# Updates in Linear:
# - Detailed technical comments
# - Status changes
# Updates locally:
# - PROGRESS.md journal entry
# - Clears completed from task-context.md

# 5. Debug an issue
/project:debug "Border placement not working"

# Creates:
# - Reproduction test first
# - Five Whys root cause analysis
# - Bug issue in Linear with full investigation

# 6. Direct Linear operations
/project:linear-task create --title "Add caching to BorderRenderer" --parent PRJ-17

# Direct Linear management:
# - Create/update/query tasks
# - Add comments and attachments
# - List tasks by various criteria
```

## Benefits of Linear-First Approach

1. **Single Source of Truth**: No confusion about which document is current
2. **Real-time Collaboration**: Team sees updates immediately
3. **Full Traceability**: Complete history in one place
4. **Better Search**: Linear's search beats file grep
5. **Mobile Access**: Work from anywhere
6. **Reduced File Management**: No more docs/specs folder sprawl
7. **Integrated Workflow**: Issues, comments, and status in one tool

## Migration from Local Files

For existing projects:
1. Local spec files can be imported to Linear as needed
2. PROGRESS.md continues as development journal
3. No need to migrate old files - start Linear-first going forward
4. Reference old files in Linear issues if needed

## Note on Commands

All project management flows through the core workflow commands listed above.

This approach maximizes the value of Linear while maintaining just enough local context for efficient development.
