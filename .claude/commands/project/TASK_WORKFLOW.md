# Complete Project Development Workflow

This document describes the complete, self-contained development workflow for HexChitEngine using Linear as the single source of truth and Test-Driven Development (TDD) throughout.

## Overview

The workflow follows a continuous cycle:

```
Feature Request → Requirements → Sprint Planning → Design → Task Implementation → Review → Next Task
                                                                    ↑                           ↓
                                                                    └───────────────────────────┘
```

Alternative entry for bugs:
```
Bug Report → Debug Analysis → Task Implementation → Review
```

## Complete Development Cycle

### Phase 1: Feature Initialization

**Command:** `/project:init-feature <feature-name>`

**What it does:**
1. Gathers requirements in EARS format
2. Creates Linear epic and requirement issues
3. Optionally scaffolds common code patterns
4. Hands off to sprint planning

**Output:**
- Linear epic created with requirements
- Optional scaffolded components with tests
- Clear next step: sprint planning

**Example:**
```bash
/project:init-feature "Inventory System"
# Creates requirements, optionally scaffolds InventorySystem
# Suggests: /project:sprint-planning PRJ-42
```

### Phase 2: Sprint Planning & Design

**Command:** `/project:sprint-planning [epic-id]`

**What it does:**
1. Creates sprint cycles in Linear
2. Breaks down epics with interactive grooming
3. Creates technical design documents
4. Generates TDD-ready implementation tasks
5. Assigns tasks to sprints

**Output:**
- Sprint cycles created
- Technical design approved
- Implementation tasks ready
- Clear next step: task implementation

**Example:**
```bash
/project:sprint-planning PRJ-42
# Plans sprints, creates design, breaks down tasks
# Suggests: /project:do-task PRJ-43
```

### Phase 3: Task Implementation (TDD)

**Command:** `/project:do-task <task-id>`

**What it does:**
1. Validates context and dependencies
2. Updates Linear status to "In Progress"
3. Optionally scaffolds new components
4. Follows strict TDD cycle:
   - RED: Write failing test
   - GREEN: Make test pass
   - REFACTOR: Improve code
5. Runs code quality checks
6. Commits changes locally
7. Updates status to "In Review"

**Output:**
- Task implemented with full test coverage
- All tests passing
- Changes committed (not pushed)
- Clear next step: code review

**Example:**
```bash
/project:do-task PRJ-43
# Implements task with TDD
# Suggests: /project:review PRJ-43
```

### Phase 4: Code Review

**Command:** `/project:review <task-id>`

**What it does:**
1. Runs all tests and quality checks
2. Reviews code against standards
3. If passed:
   - Updates Linear to "Done"
   - Pushes changes to remote
   - Suggests next task
4. If failed:
   - Documents issues
   - Guides back to implementation

**Output:**
- Task completed or feedback provided
- Next task suggested
- Option for broader review

**Example:**
```bash
/project:review PRJ-43
# Reviews and completes task
# Suggests: /project:do-task PRJ-44
```

## Alternative Entry Points

### Bug Fixes

**Command:** `/project:debug <issue-description>`

**What it does:**
1. Implements Five Whys analysis
2. Creates reproduction test
3. Identifies root cause
4. Creates Linear bug issue
5. Flows into normal task implementation

**Example:**
```bash
/project:debug "Units can move through walls"
# Analyzes bug, creates issue
# Continues with: /project:do-task PRJ-45
```

### Direct Task Work

If you already have a Linear task ID:
```bash
/project:do-task PRJ-46
# Jumps directly to implementation
```

### Sprint/Project Reviews

For broader assessment:
```bash
/project:review sprint    # Review current sprint
/project:review milestone # Review milestone progress
/project:review          # Full project review
```

## Key Files and Their Roles

1. **task-context.md** - Temporary working memory
   - Current task information
   - Session notes
   - Cleared between features

2. **Linear** - Single source of truth
   - All requirements
   - All designs
   - All tasks
   - All progress tracking

3. **Git** - Code versioning
   - Commits reference Linear IDs
   - Local commits during work
   - Push only after review passes

## Best Practices

1. **Always Start with Requirements**
   - No coding without Linear issues
   - Requirements drive design
   - Design drives tasks

2. **Follow TDD Strictly**
   - Red → Green → Refactor
   - Never skip test writing
   - Tests define behavior

3. **Use Scaffolding When Appropriate**
   - For new systems/components
   - Provides TDD structure
   - Maintains consistency

4. **Complete One Task at a Time**
   - Focus on single Linear issue
   - Full implementation and review
   - Then move to next

5. **Keep Linear Updated**
   - Real-time status updates
   - Document decisions in comments
   - Track all progress

## Workflow Commands Summary

```bash
# Start new feature
/project:init-feature "Feature Name"

# Plan sprints and create tasks  
/project:sprint-planning PRJ-XX

# Implement task with TDD
/project:do-task PRJ-XX

# Review and complete task
/project:review PRJ-XX

# Debug issue
/project:debug "Problem description"

# Utility commands
/project:linear-task    # Create Linear issues
/project:update        # Sync progress
```

## Success Metrics

- All code has tests written first
- Linear reflects real-time status
- Every commit references Linear ID
- Reviews pass before pushing
- Clear handoffs between phases

This workflow ensures consistent, high-quality development with full traceability and TDD practices throughout the entire cycle.