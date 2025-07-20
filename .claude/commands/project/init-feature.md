# /project:init-feature - Initialize New Feature

Initialize a new feature with requirements gathering and Linear integration.

## Usage
```
/project:init-feature <feature-name>
```

## Workflow Overview

This command starts the complete development cycle for a new feature:
1. Requirements gathering in Linear
2. Handoff to sprint planning for design and task breakdown

**IMPORTANT:** Follow each step and use the TodoWrite tool to track progress.

### Step 1: Requirement Gathering

You are an experienced project planner working with the user on implementing requirements for the $ARGUMENTS feature. 

Ultrathink.

First, generate an initial set of requirements in EARS format based on the feature idea, then iterate with the user to refine them until they are complete and accurate. Linear is the exclusive source of truth for all requirements.

Don't focus on code exploration in this phase. Instead, just focus on writing requirements which will later be turned into a design.

**Linear-First Process:**

1. **Read the methodology:** Reference .claude/commands/project/TASK_WORKFLOW.md for complete guidance
2. **Check Current Project Context:**
   - Use `mcp__linear__list_projects` to see active projects
   - Use `mcp__linear__list_teams` to identify the HexChitEngine team
   - Identify the appropriate project for this feature (usually "HexChitEngine Editor Development")

2. **Create Feature Epic:**
   - Run `/project:linear-task.md` to create epic. Documentation on how to do this is within the file.
   - Mark priority based on feature importance (1=Urgent, 2=High, 3=Medium, 4=Low)
   - Add comprehensive description with feature overview

3. **Create Requirement Issues:**
   - For each major requirement area, create a Linear issue as a sub-task
   - Use the epic as the parent issue
   - Include user stories and acceptance criteria in the issue description

4. **Create Minimal Context File:**
   - Create `task-context.md` in project root with:
     - Feature name and Linear epic ID
     - List of requirement Linear IDs
     - Quick reference for current work session
   - This file is temporary and can be regenerated from Linear

**Constraints:**

- The model MUST first check Linear for existing related issues using `mcp__linear__list_issues` with appropriate filters
- The model MUST create a parent epic issue in Linear for the feature
- The model MUST create individual requirement issues in Linear for each major requirement area
- The model MUST NOT create docs/specs folders or requirement.md files - Linear is the source of truth
- The model MUST format each Linear issue with:
  - A clear title summarizing the requirement
  - A description containing:
    - User story in the format "As a [role], I want [feature], so that [benefit]"
    - Numbered list of acceptance criteria in EARS format
  - Appropriate labels (e.g., "requirement", "feature", "editor", etc.)
  - Links to related issues when applicable
  
- Example Linear issue format:
  ```
  Title: Player Inventory System - Core Functionality
  
  Description:
  ## User Story
  As a player, I want to manage my inventory, so that I can organize and use items during gameplay.
  
  ## Acceptance Criteria
  1. WHEN the player opens the inventory UI, THEN the system SHALL display all items in a grid layout
  2. WHERE the inventory is full, IF the player attempts to pick up an item, THEN the system SHALL display a "Inventory Full" message
  3. WHILE the player is dragging an item, the system SHALL highlight valid drop locations
  4. WHEN the player right-clicks an item, THEN the system SHALL display a context menu with available actions
  5. IF the player drops an item outside the inventory bounds, THEN the system SHALL return the item to its original position
  
  ## Technical Considerations
  - Grid size: 8x6 slots
  - Item stacking support
  - Drag-and-drop implementation
  - Save/load persistence
  ```

- The model SHOULD consider edge cases, user experience, technical constraints, and success criteria in the initial requirements
- The model MUST create/update `task-context.md` with Linear issue IDs for quick reference during work
- After creating all requirement issues in Linear, the model MUST provide a summary with links to the created issues
- The model MUST ask the user "Do the requirements look good? I've created [N] requirement issues in Linear. If these look good, we can move on to the design phase." and wait for response
- The model MUST update Linear issues if the user requests changes
- The model MUST ask for explicit approval after every iteration of edits
- The model MUST NOT proceed to the design document until receiving clear approval
- The model SHOULD use Linear's comment feature to track requirement discussions
- The model MAY suggest using Linear's label system to categorize requirements (e.g., "must-have", "nice-to-have", "technical-debt")

### Step 2: Handoff to Sprint Planning

After requirements are approved and any scaffolding is complete:

1. **Update task-context.md** with:
   ```markdown
   # Task Context - [Feature Name]
   
   **Feature Epic:** [Linear Epic ID]
   **Requirements:** [List of requirement issue IDs]
   **Scaffolded Components:** [If any]
   **Session Started:** [YYYY-MM-DD HH:MM]
   
   ## Next Steps
   - Run /project:sprint-planning to create technical design
   - Break down into implementation tasks
   ```

2. **Inform the user:**
   ```
   ✅ Requirements complete and approved in Linear!
   Epic: [PRJ-XX] - [Feature Name]
   Requirements: [N] issues created
   [Scaffolding: Created X components] (if applicable)
   
   Next step: Run /project:sprint-planning [PRJ-XX] to:
   - Create technical design document
   - Plan sprints and break down tasks
   - Begin implementation workflow
   ```

**Linear MCP Commands Reference:**
- `mcp__linear__list_projects` - List all projects
- `mcp__linear__create_issue` - Create new requirement issues
- `mcp__linear__update_issue` - Update existing requirements
- `mcp__linear__list_issues` - Search for existing related issues
- `mcp__linear__create_comment` - Add comments to requirement discussions
- `mcp__linear__list_issue_labels` - Get available labels for categorization
