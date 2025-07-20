# /project:review - Comprehensive Project Review

Perform a comprehensive project review using Linear as the source of truth for all project state, architecture, and progress tracking.

## Usage
```
/project:review [scope]
```

Where scope can be:
- `TASK-ID` (e.g., PRJ-42) - Review specific task (most common from do-task)
- Empty - Full project review
- `milestone` - Current milestone review
- `sprint` - Current sprint review
- `architecture` - Architecture review

## Review Process

### Task-Level Review (from do-task)

When reviewing a specific task (most common flow):

1. **Verify Task Status**
   - Use `mcp__linear__get_issue` to get task details
   - Confirm status is "In Review"
   - Check implementation completeness

2. **Run Automated Checks**
   - Execute all tests
   - Check test coverage
   - Verify no regression in existing tests

3. **Code Quality Review**
   - Adherence to project conventions
   - Proper error handling
   - Performance considerations
   - Security best practices

4. **Handle Review Results**
   - **PASS**: 
     - Update Linear status to "Done"
     - Commits are ready (already on main branch)
     - Suggest next task from sprint
   - **FAIL**:
     - Document issues in Linear
     - Keep status as "In Review"
     - Guide user back to `/project:do-task [TASK-ID]` for fixes

5. **Handoff**
   ```
   ✅ Code Review Complete!
   
   Task: [PRJ-XX] - [Task Title]
   Result: PASSED ✓
   
   - Linear status updated to Done
   - All changes committed to main branch
   - Ready for next task
   
   Next task suggestion: [PRJ-YY] - [Next Task Title]
   Run: /project:do-task [PRJ-YY]
   
   Or review sprint progress: /project:review sprint
   ```

### Full Project/Sprint Review

The review follows a structured 9-step process tracked via TodoWrite tool:

### 1. Analyze Review Scope and Context
- Parse the scope argument to determine review focus
- Query Linear for current project state:
  - Active epics and their status
  - Current milestone (if using milestones in Linear)
  - Sprint/iteration status (if using cycles in Linear)
  - Recent activity and velocity

### 2. Execute and Assess Test Infrastructure Health
- Run comprehensive test suite
- Calculate test health score (0-10 scale):
  - 10: 100% pass rate, no infrastructure issues
  - 8-9: >95% pass rate, minor issues only
  - 6-7: 80-95% pass rate, some non-critical failures
  - 4-5: 60-80% pass rate, significant issues
  - 0-3: <60% pass rate, critical infrastructure problems
- Categorize failures:
  - Infrastructure: Import errors, missing modules
  - Configuration: Environment setup issues
  - Logic: Assertion failures, actual bugs
  - Flaky: Intermittent failures
- Determine blocking status:
  - Score < 6: BLOCKS epic progression
  - Score < 8: BLOCKS milestone completion
  - Score < 4: TRIGGERS emergency response

### 3. Assess Architecture Documentation Alignment
- Review architecture documentation in docs/ADRs/ and docs/technical-design-document.md
- Compare documented architecture with current implementation
- Use Zen MCP server for comprehensive analysis:
  ```
  mcp__zen__analyze with model="gemini-2.5-pro" 
  analysis_type="architecture"
  step="Verify architecture implementation matches documentation"
  ```
- Identify deviations and technical debt

### 4. Review Progress Against Linear Epics
- Query Linear for epic status and progress:
  ```
  mcp__linear__list_issues --projectId <project> --filter epic
  ```
- For each active epic:
  - Check completion percentage
  - Review blocked issues
  - Assess velocity trends
  - Identify at-risk deliverables

### 5. Analyze Codebase Structure and Quality
- Use Zen MCP server for comprehensive analysis:
  ```
  # For code quality review
  mcp__zen__codereview with model="gemini-2.5-pro"
  review_type="full"
  step="Review codebase quality and patterns"
  
  # For architecture analysis
  mcp__zen__analyze with model="gemini-2.5-pro"
  analysis_type="architecture" 
  step="Analyze system architecture and dependencies"
  ```
- Focus areas:
  - Directory organization and separation of concerns
  - Dependency management and coupling
  - Consistent patterns across systems
  - Code duplication and technical debt

### 6. Audit Development Workflow Compliance
- Check adherence to documented workflows:
  - TDD practices (test-first development)
  - Linear issue tracking (proper status updates)
  - Git commit conventions (Linear ID references)
  - Code review process
- Verify no hardcoded credentials or keys
- Check for proper environment configuration

### 7. Evaluate Technical Decisions and Complexity
- Review recent technical decisions in Linear comments
- Assess complexity vs business value ratio
- Identify over-engineering or premature optimization
- Check for scalability concerns
- Evaluate library and framework choices

### 8. Provide John Carmack-Style Technical Critique
Focus on simplicity, performance, and pragmatism:
- **Simplicity**: Are we solving problems in the most straightforward way?
- **Performance**: Are there obvious bottlenecks or inefficiencies?
- **Maintainability**: Will a new developer understand this in 6 months?
- **Robustness**: How does the system handle edge cases?
- **Technical Debt**: What shortcuts will hurt us later?

Be brutally honest but constructive, considering project constraints and goals.

### 9. Generate Comprehensive Review Report
Create a detailed review report as a Linear issue:

```markdown
# Project Review - [Date]

## 🎯 Executive Summary
- **Overall Health**: EXCELLENT | GOOD | NEEDS_ATTENTION | CRITICAL
- **Test Infrastructure**: [Score]/10
- **Progress Status**: ON_TRACK | AT_RISK | BLOCKED
- **Architecture Alignment**: ALIGNED | MINOR_DRIFT | MAJOR_CONCERNS

## 📊 Metrics
- Test Pass Rate: X%
- Epic Completion: Y%
- Velocity Trend: ↑↓→
- Technical Debt Level: LOW | MEDIUM | HIGH

## 🔍 Key Findings

### Test Infrastructure
[Detailed test results and health assessment]

### Architecture & Code Quality
[Key observations from architecture review]

### Progress & Delivery
[Epic status and risk assessment]

### Workflow Compliance
[Process adherence observations]

## 🔥 John Carmack Critique
[Top 3 brutally honest technical observations]

## 🚨 Critical Issues
[Must-fix problems with severity ratings]

## 💡 Recommendations

### Immediate Actions
1. [Critical fixes needed now]

### Short-term Improvements
1. [1-2 sprint improvements]

### Long-term Considerations
1. [Architecture evolution needs]

## 📈 Next Steps
- [ ] Address critical issues
- [ ] Update architecture docs if needed
- [ ] Plan remediation tasks
```

## Linear Integration

The review creates the following in Linear:

1. **Review Issue**: 
   - Title: "Project Review - [Date] - [Overall Health]"
   - Labels: `review`, `documentation`
   - Full review content in description

2. **Action Items**:
   - Create sub-issues for critical findings
   - Link to relevant epics
   - Assign appropriate priorities

3. **Comments**:
   - Add review summary to relevant epics
   - Note any blocking issues

## MCP Commands Used

```typescript
// Linear queries
mcp__linear__list_issues({ projectId, filter: "epic" })
mcp__linear__get_issue({ id: epicId })
mcp__linear__create_issue({ 
  title: "Project Review",
  description: reviewContent,
  labels: ["review"]
})
mcp__linear__create_comment({ issueId, body: summary })

// Documentation review
grep -r "architecture" docs/

// Test execution
/test:run --all
/test:coverage-analysis

// Architecture and code quality analysis
mcp__zen__codereview({
  model: "gemini-2.5-pro",
  review_type: "full",
  step: "Comprehensive code quality review",
  relevant_files: ["src/", "test/"]
})

mcp__zen__analyze({
  model: "gemini-2.5-pro", 
  analysis_type: "architecture",
  step: "System architecture and dependency analysis"
})


## Review Frequency

- **Full Review**: Monthly or at milestone boundaries
- **Sprint Review**: End of each sprint/cycle
- **Architecture Review**: When major changes proposed
- **Emergency Review**: When test score < 4

## Best Practices

1. **Always start with test health** - It's a leading indicator
2. **Use Zen MCP for large analysis** - Provides structured workflows for code review and architecture analysis
3. **Document in Linear** - Single source of truth
4. **Create actionable items** - Reviews should drive improvement
5. **Track trends** - Compare with previous reviews

## Example Usage

```bash
# Full project review
/project:review

# Current sprint only
/project:review sprint

# Architecture focus
/project:review architecture

# Specific epic
/project:review epic:EPIC-123

# Specific task
/project:review TASK-44
```

This ensures all project reviews are tracked in Linear with actionable outcomes and maintains your Linear-first methodology.
