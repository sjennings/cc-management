Before we do anything - read the instructions below

I would like to write an documented plan on how you'll follow instructions below, you are not to proceed until I have reviewed your planning, then and only then will I allow you to proceed when you have created an plan and an Todo that sufficiently indicates you understand the prompt.

Create a recursive multi-agent coordination system using up to 12 specialized subagents to comprehensively review and improve this codebase. The system must utilize existing CLAUDE.md files and create new, subdirectory-specific CLAUDE.md files to build a persistent, evolving knowledge base.

Requirements: Layered Execution Architecture: For each layer below, define three distinct subagent roles with clear responsibilities, inputs, and outputs.

Layer 1 (Foundation): The goal of this layer is to analyze the codebase's foundational structure, quality, and test posture.

Layer 2 (Intelligence): The goal of this layer is to perform deeper analysis, identifying patterns, performance bottlenecks, and security vulnerabilities based on the foundation layer's findings.

Layer 3 (Execution): The goal of this layer is to act upon the intelligence gathered, performing tasks like refactoring, test generation, and documentation.

Layer 4 (Verification): The goal of this layer is to validate all changes, audit for production readiness, and ensure the system's goals have been met without regressions.

Cross-Mapping Protocol & Shared State:

Create and maintain a shared state file named project_coordination_state.json. All agents must read from and write to this file.

The shared state JSON must follow this precise structure, which you will populate with the agent roles you define:

JSON

{ "coordination_metadata": { "project_name": "PROJECT_NAME_HERE", "phase": "recursive-coordination", "start_time": "2025-07-01T10:57:38Z", "current_layer": 1, "status": "active" }, "agent_registry": { "L1A1": { "name": "ROLE_TO_BE_DEFINED_BY_USER_OR_AI", "layer": 1, "status": "pending", "started_at": null, "completed_at": null, "findings_count": 0, "dependencies": [], "outputs": ["output_file.json"] }, "L1A2": { "name": "ROLE_TO_BE_DEFINED_BY_USER_OR_AI", "layer": 1, "status": "pending", "dependencies": ["L1A1"], "outputs": ["another_output.json"] } }, "shared_findings": { "codebase_insights": {}, "quality_metrics": {}, "cross_agent_discoveries": {} }, "cross_mapping": { "agent_dependencies": { "L2A1": ["L1A1", "L1A2"] }, "data_flows": { "L1A1->L2A1": ["structure_map", "dependency_graph"] } }, "coordination_state": { "blocking_issues": [], "completed_tasks": [], "pending_validations": [], "layer_checkpoints": { "layer_1": "pending", "layer_2": "pending", "layer_3": "pending", "layer_4": "pending" } } } CLAUDE.md Context Strategy:

Reference the main project CLAUDE.md for overall coordination rules.

CREATE new, subdirectory-specific CLAUDE.md files tailored to the contents of each directory and its subdirectories (e.g., packages/*/CLAUDE.md, tests/CLAUDE.md).

Each new CLAUDE.md must include directory-specific instructions, local conventions, relevant patterns, and @imports to shared documentation.

Update the main project CLAUDE.md with @imports that reference all newly created subdirectory CLAUDE.md files, creating a unified context tree.

Recursive Cycles & Validation:

Each layer must build upon the validated findings from the previous layers.

Agents within a layer can run in parallel but must sync and have their work validated before the next layer begins.

All claims of improvement must be backed by evidence (e.g., benchmark results, test coverage reports, static analysis scores). No estimates are allowed.

Codebase Review Focus:

Perform a complete structural analysis and generate a dependency map.

Identify and triage quality issues related to tests, linting, performance, and security.

Execute refactoring tasks and ensure all changes are validated against regressions.

Generate comprehensive documentation based on findings.

Execution Instructions: Initialization:

First, define the specific roles for all 12 agents according to the layered architecture.

Then, create the project_coordination_state.json file. Populate the agent_registry and cross_mapping sections with the roles and dependencies you have just defined.

Phase 1 - Foundation Layer:

Deploy the three Layer 1 agents.

One of the defined Layer 1 agents must be tasked with mapping the codebase structure and then creating the initial set of CLAUDE.md files in each major subdirectory. This agent must also update the main CLAUDE.md with the corresponding @imports.

Agents must update their status in the agent_registry and populate the shared_findings section of the JSON file as they work.

Ongoing Operation:

All subsequent agents (Layers 2-4) must first query the project_coordination_state.json to understand the current state and review findings from previous agents.

Before starting, each agent must verify that its defined dependencies are met.

Agents must adhere strictly to the instructions found in the CLAUDE.md files within their current working directory.

As new, significant patterns or conventions are established, agents must update or create CLAUDE.md files to persist this knowledge.
