# Sprint Planning and Grooming Process

This document defines the process for planning and grooming our development sprints. The goal is to ensure that all planned work is relevant, well-understood, and broken down into actionable tasks, fostering a shared technical understanding before development begins.

---

## 1. Sprint Planning Meeting

**Objective**: Define the overall goals and scope for the upcoming sprint.

**Participants**: Product Owner (you), Engineering Lead (you), AI Assistant (me)

**Process**:
1.  **Review High-Level Roadmap**: Discuss the strategic priorities from `ACTION-PLAN.md` and `docs/04-Execution/02-Product-Roadmap.md`.
2.  **Select Epics**: Identify the epics from the product backlog that align with the sprint's goals and fit within the estimated sprint capacity.
3.  **Define Sprint Goal**: Articulate a clear, concise goal for the sprint.
4.  **Create Sprint Folder**: Create a new directory `sprints/<sprint_number>/` (e.g., `sprints/2/`).
5.  **Create Epic Files**: For each selected epic, create a new markdown file `sprints/<sprint_number>/<epic_name>.md`.
6.  **Initial Epic Population**: Populate each epic file with its `Description` and initial `User Stories` (if known).

---

## 2. Epic Grooming (Iterative Discussion)

**Objective**: Break down each epic into detailed, actionable tasks, ensure relevance, and establish a shared technical understanding. This is an iterative process involving discussion and refinement.

**Participants**: Product Owner (you), AI Assistant (me)

**Process**:
For each epic in the current sprint:
1.  **Product Owner Review**: You, as the Product Owner, review the epic's `Description` and `User Stories`.
2.  **AI Assistant Questioning**: I will ask a series of clarifying questions to:
    *   **Ensure Relevance**: Confirm the epic's alignment with sprint goals and overall MVP.
    *   **Clarify Scope**: Pinpoint what's in and out of scope.
    *   **Build Technical Baseline**: Uncover potential technical challenges, dependencies, and design considerations.
    *   **Identify Edge Cases**: Prompt thinking about unusual scenarios or error conditions.

    **Example Questions I might ask**:
    *   **Relevance/Value**: "How does this epic directly contribute to our current MVP success metrics (e.g., IAM Hell Visualizer, core dependency mapping)? What specific user pain does it alleviate?"
    *   **User Stories**: "Are these user stories truly from the user's perspective? Do they capture the 'why' behind the 'what'? Can we add acceptance criteria to each story?"
    *   **Technical Deep Dive**: "What are the primary technical challenges you foresee in implementing this? Are there any external services or APIs we'll need to integrate with? What are the potential performance implications?"
    *   **Dependencies**: "Does this epic depend on any other epics in this sprint or future sprints? Are there any external teams or resources we'll need?"
    *   **Edge Cases/Error Handling**: "What happens if [X unexpected scenario] occurs? How should the system behave? What kind of error messages should the user see?"
    *   **Data Model Impact**: "How will this epic impact our Neo4j data model? Are there new node types, relationship types, or properties required?"
    *   **Testing Strategy**: "What specific types of tests (unit, integration, end-to-end) will be critical for this epic? Are there any complex scenarios that will be difficult to test?"

3.  **Task Breakdown**: Based on our discussion, we will break down each `User Story` into granular `Tasks`. Each task should be:
    *   **Actionable**: Clearly define what needs to be done.
    *   **Estimable**: Small enough to provide a reasonable time estimate.
    *   **Testable**: Have clear acceptance criteria.

4.  **Low-Level Details**: For each `Task`, we will include:
    *   `Estimate`: Time required (e.g., in hours).
    *   `Dependencies`: Any other tasks or external factors it relies on.
    *   `Acceptance Criteria`: How we know the task is complete and correct.
    *   `Notes`: Any technical considerations, design choices, or open questions.

5.  **Document Update**: The epic markdown file (`sprints/<sprint_number>/<epic_name>.md`) is updated directly during or immediately after the grooming session.

---

## 3. Sprint Kickoff

**Objective**: Ensure the entire development team understands the sprint goals and the details of each epic, and commits to the work.

**Participants**: Product Owner, Engineering Lead, Development Team

**Process**:
1.  **Review Sprint Goal**: Reiterate the sprint's overall objective.
2.  **Epic Presentations**: Each Epic Owner (or you, initially) briefly presents their groomed epic, highlighting:
    *   The `Description` and `User Stories`.
    *   Key `Tasks` and their `Acceptance Criteria`.
    *   Any significant `Dependencies` or technical considerations.
3.  **Q&A**: The team asks clarifying questions to ensure a shared understanding.
4.  **Commitment**: The team commits to delivering the work in the sprint.
5.  **Task Assignment**: Tasks are assigned to individual developers or pairs.

---

## Epic Document Structure (Example)

```markdown
# Epic: <Epic Title>

**Sprint**: <Sprint Number>
**Status**: Not Started | In Progress | Done
**Owner**: <Developer Name(s)>

---

## Description

<A detailed description of the epic and its purpose.>

## User Stories

- [ ] **Story 1:** <User story description>
    - **Tasks:**
        - [ ] <Task 1 description> (Estimate: <time>, Dependencies: <list>, Acceptance Criteria: <criteria>, Notes: <notes>)
        - [ ] <Task 2 description> (Estimate: <time>, Dependencies: <list>, Acceptance Criteria: <criteria>, Notes: <notes>)
        - ...
- [ ] **Story 2:** <User story description>
    - **Tasks:**
        - [ ] <Task 1 description> (Estimate: <time>, Dependencies: <list>, Acceptance Criteria: <criteria>, Notes: <notes>)
        - ...

## Dependencies

- <List any dependencies on other epics or external factors>

## Acceptance Criteria (Overall Epic)

- <List the overall criteria that must be met for the epic to be considered complete>
```
