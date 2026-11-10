# Agent team

For Mona's Project Pulse dashboard, I will use a four-person custom agent team defined under the repository's agent folder and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 5.5 (copilot). Responsibility: research the codebase and requirements, identify edge cases, and produce a staged implementation plan with file ownership and validation expectations. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsibility: break the work into phases, delegate to specialist agents, manage sequencing, and verify the integrated result before handing it back. Definition: `.github/agents/orchestrator.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement application logic, bug fixes, and any required runnable app support, such as the dashboard launch configuration for Project Pulse. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsibility: shape the dashboard's user experience, accessibility, visual design, layout, and responsive behavior so the frontend feels like a polished Project Pulse product. Definition: `.github/agents/designer.agent.md`.

The team is coordinated with GitHub Copilot CLI in a Codespace, with the Orchestrator acting as the project lead and the Planner, Coder, and Designer agents handling the execution plan, implementation, and product polish respectively.
