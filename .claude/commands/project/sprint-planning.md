# /project:sprint-planning - Sprint Planning from Milestones

Plan and groom sprints from Linear milestones using interactive epic breakdown.

<instructions>
CRITICAL: All planning happens in Linear! Milestones → Cycles → Epics → Tasks

## Sprint Planning Process

### 1. Identify Planning Context

**If no argument provided:**
- Use `mcp__linear__list_issues` with milestone label filter to show milestones
- Ask user which milestone to plan sprints for

**If milestone ID provided (e.g., PRJ-5):**
- Use `mcp__linear__get_issue` to retrieve milestone details
- Check milestone status and timeline

### 2. Create Sprint Cycles

**Check Existing Cycles:**
- Use `mcp__linear__list_cycles` to see current/upcoming cycles
- Identify gaps in sprint planning

**Create New Cycles for Milestone:**
- Break milestone timeline into 2-week sprints
- Use `mcp__linear__create_issue` to create cycle planning issues
- Link cycles to parent milestone
- Include sprint goals and key deliverables

**Cycle Naming Convention:**
```
Sprint [N]: [Milestone] - [Focus Area]
Example: Sprint 7.1: Editor Suite - Unit Editor
```

### 3. Epic Grooming (Interactive)

For each major feature in the milestone:

**Create Feature Epics:**
- Use `mcp__linear__create_issue` with parent set to milestone
- Title: "[Feature] - Requirements & Implementation"
- Add detailed description with scope

**Grooming Questions to Ask User:**
1. **Relevance Check:**
   - How does this epic support the milestone goals?
   - Is this critical path or nice-to-have?
   - What's the priority relative to other epics?

2. **Scope Clarification:**
   - What are the core features vs stretch goals?
   - What edge cases should we handle?
   - What's explicitly out of scope?

3. **Technical Understanding:**
   - What existing systems will this integrate with?
   - Are there technical constraints to consider?
   - What's the testing strategy?

4. **Task Breakdown:**
   For each epic, create task issues with:
   - Clear implementation steps
   - Effort estimates (in hours/days)
   - Dependencies on other tasks
   - Acceptance criteria
   - Technical approach (TDD steps)

### 4. Create Technical Design (For Complex Epics)

After epic approval and before task breakdown, complex features require technical design documentation. This ensures architectural alignment and clarifies implementation details.

**Linear-First Design Process:**

1. **Link to Epic:**
   - Design is created as a sub-issue of the epic
   - Use `mcp__linear__create_issue` with parent set to epic ID
   - Title: "{Epic Name} - Technical Design"

2. **Create Design Issue:**
   - Set appropriate priority (same as parent epic)
   - Add "design" label if available
   - Include full design document content in description
   - Link to all related requirement issues

3. **Design Document Structure:**
   ```markdown
   # {Feature Name} - Technical Design
   
   **Parent Epic:** [PRJ-XX]
   **Status:** [Draft | In Review | Approved]
   
   ## Overview
   [High-level summary of the design approach]
   
   ## Architecture
   [System architecture and how this feature fits]
   
   ### Component Diagram
   ```mermaid
   graph TD
       A[Component A] --> B[Component B]
       B --> C[Component C]
   ```
   
   ## Components and Interfaces
   
   ### Component A
   - **Purpose:** [What it does]
   - **Interface:** [Public API]
   - **Implementation Task:** TBD
   
   ### Component B
   - **Purpose:** [What it does]
   - **Interface:** [Public API]
   - **Implementation Task:** TBD
   
   ## Data Models
   [Data structures and their relationships]
   
   ## Error Handling
   [Error scenarios and handling strategies]
   
   ## Testing Strategy
   [Unit, integration, and acceptance test plans]
   
   ## Implementation Tasks
   (To be created after design approval)
   1. Component A implementation
   2. Component B implementation
   3. Integration and testing
   ```

4. **Design Review Process:**
   - Use Linear comments for design discussions
   - Update issue status: Draft → In Review → Approved
   - Document design decisions and rationales in comments
   - Link research sources and references

5. **Research During Design:**
   - Identify areas needing investigation based on requirements
   - Conduct research and build context in conversation thread
   - Summarize key findings in the Linear issue
   - Cite sources and include relevant links

**Design Approval Gates:**
- Design must be reviewed before task creation
- At least one team member should review complex designs
- Update status to "Approved" before proceeding
- If gaps identified, may need to revisit requirements

### 5. Task Definition Process

**After Design Approval, Create Implementation Tasks:**

The model MUST use the following approach to convert the design into actionable tasks:

1. **Verify Design Approval:**
   - Use `mcp__linear__get_issue` to confirm design status is "Approved"
   - Get parent epic and requirement references
   
2. **Create Task Issues in Linear:**
   ```
   Convert the feature design into a series of prompts for a code-generation LLM that will implement each step in a test-driven manner. Prioritize best practices, incremental progress, and test-driven development, ensuring no big jumps in complexity at any stage. Make sure that each prompt builds on the previous prompts, and ends with wiring things together. There should be no hanging or orphaned code that isn't integrated into a previous step. Focus ONLY on tasks that involve writing, modifying, or testing code.
   ```

3. **Task Issue Format:**
   ```markdown
   Title: [Epic Name] - [Specific Task]
   Example: Unit Editor - Implement Equipment Model
   
   **Parent Epic:** [PRJ-XX]
   **Design Issue:** [PRJ-YY]
   **Requirement Refs:** [PRJ-AA, PRJ-BB]
   
   ## Objective
   [Clear objective focused on code implementation]
   
   ## TDD Approach
   1. RED: Write unit tests for [specific behavior]
   2. GREEN: Implement [feature] to pass tests
   3. REFACTOR: Clean up [specific areas]
   
   ## Implementation Details
   - [Specific technical steps]
   - [Integration points]
   - [Data structures needed]
   
   ## Files to Create/Modify
   - `src/path/to/file.cs`
   - `test/unit/path/to/fileTest.cs`
   
   ## Estimate
   [X] hours
   
   ## Dependencies
   - [PRJ-XX] must be complete
   - Requires [system/component]
   
   ## Acceptance Criteria
   - [ ] All unit tests pass
   - [ ] Feature integrates with existing code
   - [ ] Code follows project conventions
   - [ ] No regression in existing tests
   
   ## Technical Notes
   [Specific implementation guidance from design]
   ```

4. **Task Sequencing:**
   - Create tasks that build incrementally
   - Each task should produce working, tested code
   - No orphaned implementations
   - Final task wires everything together

5. **Avoid Non-Coding Tasks:**
   - NO user acceptance testing tasks
   - NO deployment tasks
   - NO performance metrics gathering
   - NO documentation tasks (except code comments)
   - NO business process tasks

### 6. Sprint Assignment

**Assign Tasks to Cycles:**
- Balance workload across sprints
- Consider dependencies between tasks
- Use `mcp__linear__update_issue` to set cycle
- Add sprint labels for filtering

**Capacity Planning:**
- ~60-80 hours per 2-week sprint (solo dev)
- Include time for code review and testing
- Buffer for unexpected issues

### 7. Grooming Completion

**Update Milestone:**
- Add comment summarizing sprint plan
- List all created cycles and epics
- Note any risks or dependencies

**Create Sprint Kickoff Issue:**
- Summary of sprint goals
- List of assigned tasks
- Key milestones and checkpoints

### 8. Handoff to Implementation

After sprint planning is complete:

1. **Update task-context.md** with current sprint info:
   ```markdown
   # Task Context - [Sprint Name]
   
   **Current Sprint:** [Cycle Name]
   **Epic:** [PRJ-XX]
   **Design:** [PRJ-YY] (Approved)
   **Tasks:** [List of task IDs]
   
   ## Ready to Start
   - First task: [PRJ-ZZ] - [Task Title]
   ```

2. **Inform the user:**
   ```
   ✅ Sprint planning complete!
   
   Sprint: [Cycle Name]
   Epic: [PRJ-XX] - [Epic Title]
   Tasks created: [N] implementation tasks
   
   Next step: Run /project:do-task [PRJ-ZZ] to:
   - Start implementing the first task
   - Follow TDD workflow
   - Track progress in Linear
   
   Or run /project:do-task to see all available tasks.
   ```

## Interactive Grooming Example

```
Assistant: I'll help you plan Sprint 7.1 for the Editor Suite milestone. 

Let's start with the Unit Editor epic. Looking at the milestone requirements:

**Relevance Questions:**
1. The Unit Editor is marked as high priority. What specific unit management features are most critical for initial content creation?
2. Should we include unit import/export in this sprint or defer to later?

**Scope Questions:**
1. For combat values, do we need full CRT integration now or just data entry?
2. Should the editor support unit variants/upgrades in v1?

**Technical Questions:**
1. The unit data model exists. Should we refactor it for editor needs?
2. What's our approach for unit icon/sprite management?

Based on your answers, I'll create a technical design issue first to ensure we have a solid architecture before breaking down tasks.

After design approval, I'll create the following tasks:
- Unit Editor UI Layout (8h)
- Equipment Model & Service (16h)
- Unit Creation Workflow (12h)
- Validation & Constraints (8h)
- Save/Load Integration (8h)

Shall I proceed with creating the design document?
```

## Best Practices

1. **Start Small:** First sprint of a milestone should be achievable
2. **Design Before Code:** Technical designs for complex features before task breakdown
3. **Dependencies First:** Technical foundations before UI
4. **Vertical Slices:** Each sprint should deliver usable features
5. **Regular Reviews:** Groom one sprint ahead
6. **Flexible Planning:** Adjust based on velocity

## Linear Commands Used

- `mcp__linear__list_issues` - Find milestones and existing work
- `mcp__linear__get_issue` - Get milestone/epic details
- `mcp__linear__create_issue` - Create cycles, epics, tasks
- `mcp__linear__update_issue` - Assign to cycles, update status
- `mcp__linear__list_cycles` - View sprint cycles
- `mcp__linear__create_comment` - Document decisions
- `mcp__linear__list_issue_labels` - Manage sprint labels
</instructions>

<workflow>
1. Parse milestone ID or show available milestones
2. Review milestone scope and timeline
3. Create sprint cycles for the milestone
4. Interactively groom each epic with the user
5. Create technical design documents for complex epics
6. Create detailed task issues based on approved designs
7. Assign tasks to appropriate sprints
8. Summarize the sprint plan
</workflow>

<constraints>
- All planning data lives in Linear only
- No local sprint folders or markdown files
- Cycles (sprints) are 2-week iterations
- Every task must have clear acceptance criteria
- Interactive grooming is required - don't assume
</constraints>