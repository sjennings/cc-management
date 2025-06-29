# AI Assistant Sprint Planning Command

This document contains the prompt to be used with an AI Assistant (e.g., Claude Code's slash command) to initiate and manage the sprint planning and grooming process.

---

**AI Assistant Directive:**

You are tasked with guiding the Product Owner through the sprint planning and grooming process for the current development sprint.

**Follow these steps:**

1.  **Identify Current Sprint**: Read the `Current Sprint` value from `/CLAUDE.md`. This is the target sprint for grooming.
2.  **Review Process**: Refer to `/docs/Sprint-Grooming-Process.md` for the detailed steps of "Epic Grooming (Iterative Discussion)".
3.  **Determine Grooming Needs**:
    *   List all epic markdown files within the `/docs/sprints/<Current Sprint>/` directory.
    *   For each epic, check its `Status` field and the completeness of its `User Stories` and `Tasks` sections. An epic needs grooming if its `Status` is `Not Started` or `In Progress` and its `Tasks` section is not yet detailed with estimates, dependencies, and acceptance criteria as per the `Epic Document Structure (Example)` in the grooming process document.
4.  **Initiate Grooming**:
    *   If there are epics identified in Step 3 that require grooming, select the next one.
    *   Begin an interactive grooming session with the Product Owner. Your primary role is to ask clarifying questions (as exemplified in Section 2 of the grooming process document) to:
        *   Ensure the epic's relevance to the MVP.
        *   Clarify its scope and identify edge cases.
        *   Build a shared technical understanding.
        *   Facilitate the breakdown of user stories into granular tasks, including `Estimate`, `Dependencies`, `Acceptance Criteria`, and `Notes`.
    *   **Propose direct updates to the epic's markdown file** (`/docs/sprints/<Current Sprint>/<epic_name>.md`) to capture all discussed details.
    *   Continue this iterative discussion until the Product Owner confirms the epic is fully groomed and ready for development.
    *   Once an epic is fully groomed, update its `Status` field in the markdown file.
5.  **Sprint Completion Check**:
    *   If all epics in the current sprint directory (`/docs/sprints/<Current Sprint>/`) have been fully groomed (i.e., their `Status` is updated and tasks are detailed), inform the Product Owner that the sprint is ready for kickoff.
    *   Ask the Product Owner if they would like to proceed with setting up the development environment (referencing Sprint 1 tasks) or move to planning the next sprint.