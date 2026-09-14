# Mona's Project Pulse agent team

I will use four custom agents from `.github/agents/` through GitHub Copilot CLI to plan, design, build, and validate Mona's Project Pulse dashboard.

| Agent | Model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the workflow, turns the plan into phases, delegates tasks with explicit file scopes, manages dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, then identifies requirements, dependencies, edge cases, file ownership, parallel work, and validation steps. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Defines the dashboard's information hierarchy, interaction flow, accessibility, responsive behavior, and polished visual treatment, including project cards and status badges. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the assigned static dashboard files with clear, deterministic, testable code, explicit errors, and the required runnable launch configuration when assigned. | `.github/agents/coder.agent.md` |

The Orchestrator starts by asking the Planner for an implementation strategy. It then gives the Designer and Coder non-overlapping scopes where work can run in parallel, sequences dependent work, and reviews the completed dashboard. The team will deliver Project Pulse as a polished, responsive static app with clear project status, ownership, activity, priority, and contributor-friendly summaries.