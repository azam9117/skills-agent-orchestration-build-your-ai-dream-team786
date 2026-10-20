# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the Planner, Designer, and Coder; divides work into phases with clear file ownership, manages dependencies and parallel work, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository, documentation, dependencies, and edge cases, then provides an implementation plan with file assignments, sequencing, and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Defines and implements dashboard UI/UX, accessibility, information hierarchy, interactions, visual clarity, and responsive styling within its assigned scope. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements application code and assigned runnable-app support, following repository patterns and validating the changes. | `.github/agents/coder.agent.md` |

The Orchestrator uses the Planner's plan to assign bounded tasks to the Designer and Coder, keeping overlapping file work sequential and independent work parallel where appropriate.
