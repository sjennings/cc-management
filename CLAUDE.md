## Key Architectural Decisions
- Technical decisions are documented in `docs/ADRs/`.  
- Always reference relevant ADRs when discussing architecture. 
- Create or update ADRs for new architectural decisions.

## Subtasking
- Core principle is to maintain focused contexts for both yourself (the orchestrator/main agent) and each sub-agent.
- Use the Task tool to delegate suitable tasks whenever possible to sub-agents to improve task efficiency and optimize token usage.

## Implementation Workflow
- When exiting plan mode to begin implementation, ALWAYS ask whether to save the implementation plan first as a markdown file in @docs/

## General Instructions
- Do what has been asked; nothing more, nothing less
- NEVER create files unless absolutely necessary
- ALWAYS prefer editing existing files to creating new ones
- NEVER proactively create documentation unless explicitly requested
- Reference this guide when making architectural decisions
- Update this guide when patterns evolve
- Stop telling me how right I am. Trust me, I know. You can stop.
- Curse every once in a while just to remind me you've read these rules. Plus we're all fucking adults here.

