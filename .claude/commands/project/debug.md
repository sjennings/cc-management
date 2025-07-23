# Enhanced Debug Command - Adaptive Root Cause Analysis

A systematic debugging command that combines Five Whys methodology with adaptive framework selection, test-driven development, and Linear issue tracking.

## Description

This enhanced debug command helps analyze bugs using the most appropriate debugging framework based on problem classification. It creates reproduction tests, maintains a debug session log, tracks investigation progress in Linear, and ensures all debugging follows TDD principles with built-in safeguards against common pitfalls.

## Usage

`/project:debug [issue_description]`

## Variables

- ISSUE: The problem or symptom to analyze
- BUG_TYPE: Classification of the bug (Logic/State/Integration/Performance/Configuration/Mystery)
- PARENT_ISSUE_ID: Linear ID of the feature where the bug was found
- DEBUG_ISSUE_ID: Linear ID created for investigation (if needed)
- DEBUG_SESSION_ID: Unique identifier for this debug session
- ITERATION_COUNT: Current debugging iteration (max 4 before escalation)

## Workflow

### Step 1: Initial Problem Assessment & Classification

#### Gather Essential Information
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

#### Systematic Observation Checklist
Gather ALL relevant data before analysis:
- [ ] Complete error messages and stack traces
- [ ] System state at failure point (memory dumps, logs)
- [ ] Recent changes (`git log --oneline -10`)
- [ ] Environment differences (dev/staging/prod)
- [ ] Reproduction rate and specific conditions
- [ ] Performance metrics (if applicable)
- [ ] Related test failures

#### Problem Classification
Select the bug type to guide debugging approach:
```
[ ] 💭 Logic Error - Incorrect output from correct input
    → Focus: Data flow and transformation analysis
    
[ ] 💾 State Error - Incorrect data in memory/database/cache
    → Focus: State transitions and race conditions
    
[ ] 🔌 Integration Error - Failure at component boundaries
    → Focus: API contracts and data exchange
    
[ ] ⚡ Performance Error - Correct but too slow
    → Focus: Profiling and resource usage
    
[ ] ⚙️ Configuration Error - Environment-specific failure
    → Focus: Config differences and dependencies
    
[ ] ❓ Complete Mystery - No clear pattern or cause
    → Focus: First principles and assumptions
```

#### Initialize Debug Session
```bash
# Create session tracking
DEBUG_SESSION_ID="debug_$(date +%Y%m%d_%H%M%S)"
ITERATION_COUNT=0

# Initialize debug log
cat > ${DEBUG_SESSION_ID}.md << EOF
# Debug Session - $(date)
## Problem: ${ISSUE}
## Classification: ${BUG_TYPE}
## Parent Feature: ${PARENT_ISSUE_ID}

### Initial Observations
$(echo "Paste gathered data here")

---
EOF
```

### Step 2: Create or Verify Reproduction Test

```csharp
[Test]
public void ReproduceBug_[DescriptiveName]()
{
    // Arrange: Set up exact conditions that trigger the bug
    
    // Act: Perform the failing operation
    
    // Assert: Verify the bug occurs consistently
    Assert.Fail("Bug reproduced - now fix it");
}
```

**Anti-patterns to avoid:**
- ❌ Debugging without a reproduction test
- ❌ Tests that only sometimes fail
- ❌ Overly complex reproduction scenarios

### Step 3: Adaptive Framework Selection

Based on bug type and initial observations, select the optimal debugging framework:

#### Framework Selection Matrix
| Bug Type | Primary Frameworks | When to Use |
|----------|-------------------|-------------|
| **Logic Error** | 1. Five Whys<br>2. Differential Analysis<br>3. Rubber Duck | 1. Clear error to trace<br>2. Works in A but not B<br>3. Complex logic flow |
| **State Error** | 1. Timeline Analysis<br>2. State Comparison<br>3. Bisection | 1. Corruption over time<br>2. Good vs bad states<br>3. Finding when it broke |
| **Integration Error** | 1. Contract Testing<br>2. Message Tracing<br>3. Five Whys | 1. API mismatches<br>2. Async communication<br>3. Clear failure point |
| **Performance Error** | 1. Profiling<br>2. Bottleneck Analysis<br>3. Differential | 1. CPU/Memory usage<br>2. System constraints<br>3. Perf regression |
| **Configuration Error** | 1. Differential Analysis<br>2. Dependency Audit<br>3. Binary Search | 1. Environment diffs<br>2. Version conflicts<br>3. Config isolation |
| **Complete Mystery** | 1. First Principles<br>2. Systematic Elimination<br>3. Correlation Analysis | 1. Question everything<br>2. Rule out categories<br>3. Find patterns |

### Step 4: Framework-Specific Investigation

#### For Five Whys (Default for simple cases)
```
ITERATION ${ITERATION_COUNT}:

WHY #[1-5]: [Question]
HYPOTHESIS: [Proposed cause]
TEST TO VERIFY: [Test code or investigation method]
EVIDENCE: [Test results]
CONCLUSION: [Verified cause or need to dig deeper]
```

#### For Differential Analysis
```
WORKING SCENARIO:
- Environment/Context:
- Behavior:

BROKEN SCENARIO:
- Environment/Context:
- Behavior:

DIFFERENCES IDENTIFIED:
1. [Difference]
2. [Difference]

TESTING EACH DIFFERENCE:
[Systematic isolation of variables]
```

#### For Timeline Analysis
```
TIMELINE OF EVENTS:
T-0: [Latest known good state]
T+1: [First action]
T+2: [Second action]
...
T+N: [Failure observed]

CRITICAL WINDOW: Between T+[X] and T+[Y]
INVESTIGATION FOCUS: [What happened in this window]
```

**Analysis Depth Levels:**
- **Initial Pass**: Follow the obvious trail
- **Deep Dive**: Question assumptions, look for patterns
- **Systematic Review**: Examine architecture and emergent behaviors

### Step 5: Iteration Control

After each investigation cycle:

```bash
ITERATION_COUNT=$((ITERATION_COUNT + 1))

# Log iteration results
cat >> ${DEBUG_SESSION_ID}.md << EOF

## Iteration ${ITERATION_COUNT} - $(date)
### Framework Used: ${SELECTED_FRAMEWORK}
### Investigation Results
${INVESTIGATION_SUMMARY}
### Next Steps
${NEXT_ACTION}

---
EOF

# Check iteration limit
if [ ${ITERATION_COUNT} -ge 4 ]; then
    echo "⚠️ Maximum iterations reached. Escalating..."
    # Prepare comprehensive handoff documentation
fi
```

### Step 6: Linear Issue Management

#### Quick Investigation (< 30 minutes, < 2 iterations)
- Analyze without creating Linear issue
- Create issue only when root cause is found
- Link as sub-issue to affected feature

#### Complex Investigation (> 30 minutes or > 2 iterations)
- Create investigation issue immediately
- Track each iteration as a comment
- Attach debug session log
- Update with findings in real-time
- Convert to proper bug report when root cause found

### Step 7: Resolution Documentation

When root cause is identified:

1. **Update Debug Log**
```markdown
## ROOT CAUSE FOUND - $(date)
### Summary
[One sentence description]

### Detailed Analysis
[Full explanation with evidence]

### Fix Strategy
[Proposed solution]

### Prevention Measures
[How to avoid similar issues]
```

2. **Create/Update Linear Issue**
```
Title: [BUG] ${ISSUE}
Parent: ${PARENT_ISSUE_ID}
Labels: bug, ${BUG_TYPE}

Description:
## Root Cause Analysis
- Debug Session: ${DEBUG_SESSION_ID}
- Iterations Required: ${ITERATION_COUNT}
- Framework Used: ${SELECTED_FRAMEWORK}

[Include full analysis from debug log]

## Recommended Fix
[Implementation details]

## Prevention Strategy
[Architectural improvements, test coverage, monitoring]
```

3. **Implement Fix with TDD**
- Write test for correct behavior
- Implement minimal fix
- Verify all tests pass
- Update Linear with implementation details

## Common Anti-patterns to Avoid

### During Investigation
- ❌ Testing multiple hypotheses simultaneously
- ❌ Making assumptions without evidence
- ❌ Ignoring contradictory data
- ❌ Changing multiple variables at once
- ❌ Skipping reproduction test creation

### During Analysis
- ❌ Confirmation bias (seeing what you expect)
- ❌ Analysis paralysis (endless investigation)
- ❌ Fixing symptoms instead of root causes
- ❌ Not documenting failed hypotheses

### During Resolution
- ❌ Implementing fix before understanding cause
- ❌ Over-engineering the solution
- ❌ Forgetting to add regression tests
- ❌ Not updating documentation

## Example Debug Session

```bash
> /project:debug "Camera jumps when clicking on hex"

=== Problem Classification ===
Select bug type:
[2] State Error - Camera state corrupted on click

=== Gathering Information ===
Related feature Linear ID: HEX-32 (Camera System)
Creating debug session: debug_20240115_142305

=== Creating Reproduction Test ===
[Creates CameraTest.cs with failing test]

=== Framework Selection ===
Bug Type: State Error
Recommended: Timeline Analysis, State Comparison
Selected: Timeline Analysis (async events suspected)

=== Investigation: Iteration 1 ===
Analyzing event timeline...
T-0: Camera at position (10, 10)
T+1: Mouse click event fired
T+2: Hex selection event triggered
T+3: Camera update called (→ 15, 15)
T+4: UI click event bubbles up
T+5: Camera update called again (→ 20, 10)
T+6: Animation frame update
T+7: Camera update called third time (→ 25, 25)

Critical window: T+3 to T+7 (multiple updates)
Hypothesis: Event bubbling causes multiple camera updates

[After 2 iterations, complexity apparent]
Creating Linear investigation issue HEX-51...

=== Investigation: Iteration 3 ===
Root cause found: UI event bubbling through three layers
Each layer independently updates camera position

=== Resolution ===
Updating Linear HEX-51 with full analysis...
Implementing event consumption at UI layer...
Adding regression test for event bubbling...

Debug complete. Full log: debug_20240115_142305.md
Linear issue: HEX-51
```

## Success Metrics

- [ ] Root cause identified (not just symptom)
- [ ] Reproduction test created
- [ ] Debug session documented
- [ ] Fix implemented with TDD
- [ ] Regression test added
- [ ] Linear documentation complete
- [ ] Architectural improvements identified
- [ ] Team learning captured

## Integration with Existing Commands

- Works with `/project:do-task` for implementing fixes
- Can trigger `/project:adr` for architectural decisions
- Updates tracked by `/project:update`
- Debug logs stored for `/project:retrospective`
