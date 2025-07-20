# Debug Command - Five Whys with Linear Integration

A systematic debugging command that implements the Five Whys methodology with test-driven development and Linear issue tracking.

## Description

This command helps debug issues using the Five Whys root cause analysis methodology. It creates reproduction tests, tracks investigation progress in Linear, and ensures all debugging follows TDD principles.

## Usage

`/project:debug [issue_description]`

## Variables

- ISSUE: The problem or symptom to analyze
- PARENT_ISSUE_ID: Linear ID of the feature where the bug was found
- DEBUG_ISSUE_ID: Linear ID created for investigation (if needed)

## Workflow

### Step 1: Initial Problem Assessment

Gather essential information:
```
PROBLEM STATEMENT:
- What test is failing? (or describe the issue)
- Expected behavior:
- Actual behavior:
- Test output/error message:
- Godot version:
- GoDotTest version:
- Related feature (Linear ID):
```

### Step 2: Create or Verify Reproduction Test

```csharp
[Test]
public void ReproduceBug_[DescriptiveName]()
{
    // Arrange: Set up exact conditions
    
    // Act: Perform the failing operation
    
    // Assert: Verify the bug occurs
    Assert.Fail("Bug reproduced - now fix it");
}
```

### Step 3: Five Whys Analysis

For each "why" level:
```
WHY #[1-5]: [Question]
HYPOTHESIS: [Proposed cause]
TEST TO VERIFY: [Test code to confirm hypothesis]
EVIDENCE: [Test results]
CONCLUSION: [Verified cause or need to dig deeper]
```

### Step 4: Linear Issue Management

#### Quick Investigation (< 30 minutes)
- Analyze without creating Linear issue
- Create issue only when root cause is found
- Link as sub-issue to affected feature

#### Complex Investigation (> 30 minutes or multiple whys)
- Create investigation issue immediately
- Track each "why" as a comment
- Update with findings in real-time
- Convert to proper bug report when root cause found

### Step 5: Resolution Documentation

When root cause is identified:
1. Create/update Linear bug issue with:
   - Root cause summary
   - Chain of causation (all 5 whys)
   - Recommended fix
   - Prevention strategy
   
2. Implement fix using TDD:
   - Write test for correct behavior
   - Implement minimal fix
   - Verify all tests pass
   - Update Linear with implementation details

## Implementation Process

### Phase 1: Problem Identification
```bash
# Start debugging
/project:debug "Units can move through impassable terrain"

# System prompts for context
Enter parent feature Linear ID (e.g., PRJ-42): PRJ-15
```

### Phase 2: Reproduction Test
```csharp
// Create failing test first
[Test]
public void Unit_CannotMove_ThroughImpassableTerrain()
{
    // Arrange
    var unit = CreateUnit(new HexCoordinate(0, 0));
    var impassableHex = new HexCoordinate(1, 0);
    SetTerrain(impassableHex, TerrainType.Impassable);
    
    // Act & Assert
    Assert.Throws<InvalidMoveException>(() => 
        unit.MoveTo(impassableHex));
}
```

### Phase 3: Five Whys Investigation
```
WHY 1: Why can the unit move through impassable terrain?
HYPOTHESIS: Movement validation doesn't check terrain type
TEST: Add logging to movement validation
EVIDENCE: No terrain check in MoveTo() method
CONCLUSION: Missing terrain validation

WHY 2: Why is terrain validation missing?
HYPOTHESIS: Movement system developed before terrain system
TEST: Check git history of both systems
EVIDENCE: Movement: commit abc123, Terrain: commit def456 (later)
CONCLUSION: Systems not integrated

WHY 3: Why weren't systems integrated?
HYPOTHESIS: No integration tests between systems
TEST: Search for movement+terrain tests
EVIDENCE: Zero integration tests found
CONCLUSION: Missing test coverage

WHY 4: Why are integration tests missing?
HYPOTHESIS: No clear testing strategy for system interactions
TEST: Review test organization and patterns
EVIDENCE: Only unit tests per system, no integration folder
CONCLUSION: Architectural gap in test strategy

WHY 5: Why is there an architectural gap?
HYPOTHESIS: Lack of system integration planning
TEST: Review design documents for integration points
EVIDENCE: Each system designed in isolation
ROOT CAUSE: Missing architectural documentation for system interactions
```

### Phase 4: Linear Documentation

Create bug issue in Linear:
```
Title: [BUG] Units can move through impassable terrain
Parent: PRJ-15 (Movement System)
Labels: bug, movement, terrain

Description:
## Root Cause Analysis

**Surface Symptom**: Units can move through impassable terrain
**Root Cause**: Missing architectural documentation for system interactions

### Five Whys Chain:
1. Movement validation doesn't check terrain type
2. Movement system developed before terrain system
3. No integration tests between systems
4. No clear testing strategy for system interactions
5. Lack of system integration planning

### Recommended Fix:
1. Add terrain validation to MoveTo() method
2. Create integration tests for movement+terrain
3. Document system interaction points
4. Add ADR for cross-system integration patterns

### Prevention:
- Establish integration test requirements
- Create system interaction matrix
- Add integration tests to DoD
```

## Example Debug Session

```bash
> /project:debug "Camera jumps when clicking on hex"

Gathering information...
Related feature Linear ID: PRJ-32 (Camera System)

Creating reproduction test...
[Creates CameraTest.cs with failing test]

Starting Five Whys analysis...

WHY 1: Why does camera jump on click?
Testing hypothesis: Click event triggers multiple camera updates
Evidence: Camera.SetPosition called 3 times per click
Proceeding to WHY 2...

[After 2 whys, complexity apparent]
Creating Linear investigation issue PRJ-51...
Continuing analysis with Linear tracking...

[After completing 5 whys]
Root cause found: Event bubbling through UI layers
Updating Linear PRJ-51 with full analysis...
Creating fix with TDD approach...

Debug complete. See Linear PRJ-51 for full documentation.
```

## Linear Integration Details

### Team & Project IDs
- Team ID: `YOU_NEED_TO_EDIT_THIS!`
- Project ID: `YOU_NEED_TO_EDIT_THIS_TOO!`

### Issue Creation Logic
```
IF investigation_time < 30_minutes AND root_cause_found:
    Create bug issue directly under parent feature
ELIF investigation_time >= 30_minutes OR multiple_whys_needed:
    Create investigation issue immediately
    Track progress in comments
    Convert to bug issue when root cause found
```

### Status Flow
```
Investigation Issue:
Todo → In Progress → Done (converted to bug)

Bug Issue:
Todo → In Progress → In Review → Done
```

## Best Practices

1. **Always Start with a Test**: No debugging without reproduction
2. **Document Each Why**: Full transparency in Linear
3. **Stay Focused**: One bug at a time
4. **Think Systemically**: Look for architectural issues
5. **Prevent Recurrence**: Add tests and documentation

## Integration with Existing Commands

- Works with `/project:do-task` for implementing fixes
- Updates tracked by `/project:update`

## Success Metrics

- Root cause found (not just symptom fixed)
- Reproduction test created
- Fix implemented with TDD
- Regression test added
- Linear documentation complete
- Architectural improvements identified