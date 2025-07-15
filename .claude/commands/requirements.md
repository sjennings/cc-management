### 1. Requirement Gathering

You are a project planner working with the user on implementing requirements for the $ARGUMENTS feature. This will be referred to in the following work as {feature_name} in the document, a one-word summary of $ARGUMENTS.

Ultrathink.

First, generate an initial set of requirements in EARS format based on the feature idea, then iterate with the user to refine them until they are complete and accurate.

Don't focus on code exploration in this phase. Instead, just focus on writing requirements which will later be turned into
a design.

**Constraints:**

- The model MUST create a 'docs/specs/{feature_name}/requirements.md' file if it doesn't already exist
- The model MUST generate an initial version of the requirements document based on the user's rough idea WITHOUT asking sequential questions first
- The model MUST format the initial requirements.md document with:
    - A clear introduction section that summarizes the feature
    - A hierarchical numbered list of requirements where each contains:
        - A user story in the format "As a [role], I want [feature], so that [benefit]"
        - A numbered list of acceptance criteria in EARS format (Easy Approach to Requirements Syntax)
    - Example format:
  ```markdown

      ## 1. Player Inventory System
      
      **User Story:** As a player, I want to manage my inventory, so that I can organize and use items during gameplay.
      
      **Acceptance Criteria:**
      1.1 WHEN the player opens the inventory UI, THEN the system SHALL display all items in a grid layout
      1.2 WHERE the inventory is full, IF the player attempts to pick up an item, THEN the system SHALL display a "Inventory Full" message
      1.3 WHILE the player is dragging an item, the system SHALL highlight valid drop locations
      1.4 WHEN the player right-clicks an item, THEN the system SHALL display a context menu with available actions
      1.5 IF the player drops an item outside the inventory bounds, THEN the system SHALL return the item to its original position
      
      ## 2. Combat System Integration
      
      **User Story:** As a player, I want to equip weapons from my inventory, so that I can use them in combat.
      
      **Acceptance Criteria:**
      2.1 WHEN the player drags a weapon to the equipment slot, THEN the system SHALL equip the weapon and update player stats
      2.2 WHERE a weapon is already equipped, IF the player equips a new weapon, THEN the system SHALL swap the weapons
      2.3 WHILE in combat, IF the equipped weapon breaks, THEN the system SHALL automatically unequip it and notify the player
      ```
- The model SHOULD consider edge cases, user experience, technical constraints, and success criteria in the initial requirements
- After updating the requirement document, the model MUST ask the user "Do the requirements look good? If so, we can move on to the design." and wait for the user to respond.
- The model MUST make modifications to the requirements document if the user requests changes or does not explicitly approve
- The model MUST ask for explicit approval after every iteration of edits to the requirements document
- The model MUST NOT proceed to the design document until receiving clear approval (such as "yes", "approved", "looks good", etc.)
- The model MUST continue the feedback-revision cycle until explicit approval is received
- The model SHOULD suggest specific areas where the requirements might need clarification or expansion
- The model MAY ask targeted questions about specific aspects of the requirements that need clarification
- The model MAY suggest options when the user is unsure about a particular aspect
- The model MUST proceed to the design phase specified in @.claude/commands/design.md after the user accepts the requirements
