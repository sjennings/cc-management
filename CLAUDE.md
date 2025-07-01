## The Prime Directive
- You cost $200 a month. You must justify that expense with efficiency and productivity.

## Documentation
- Project documentation is in `docs/`
- Design specs and technical design documents are in `docs/DDs`
- Research material is in `docs/Research`
- Historical plan implementation is in `docs/Plans`
- Project progress is in `docs/PROGRESS.md`. ALWAYS update this document FREQUENTLY.

## Key Architectural/Technical Decisions
- Technical decisions are documented in `docs/ADRs/`.  
- Always reference relevant ADRs when discussing architecture. 
- Create or update ADRs for new architectural decisions.

## Context Management
- ALWAYS monitor your available remaining context.
- Before beginning ANY action, if your available remaining context is 10% or less, write information necessary to continue your task to `docs/CONTEXT.md`. Then prompt me to /clear, followed by /init_context.

## Subtasking
- Core principle is to maintain focused contexts for both yourself (the orchestrator/main agent) and each sub-agent.
- Use the Task tool to delegate suitable tasks whenever possible to sub-agents to improve task efficiency and optimize token usage.

## Implementation Workflow
- When exiting plan mode to begin implementation, ALWAYS ask whether to save the implementation plan first as a markdown file in @docs/

## Coding Decisions
- Detailed code style and rules is in `docs/CODE_STYLE.md`
- Code must ALWAYS be under 1000 lines and human-readable.
- Always explain with comments why a given coding decision was taken.
- If a given code file approaches this limit, decompose it into logical components.
- Note debug prints with DEBUG: comments, and remove them when no longer needed.
- NEVER create code files unless absolutely necessary.
- ALWAYS prefer editing existing code files to creating new ones.

## General Instructions
- Do what has been asked; nothing more, nothing less.
- Stop telling me how right I am. Trust me, I know. You can stop.
- Curse every once in a while just to remind me you've read these rules. Plus we're all fucking adults here.

