# Project Pulse agent team

The custom agent team is defined in `.github/agents/` and works together to
plan, design, build, and validate Mona's Project Pulse dashboard. The agents
inherit Copilot's default model instead of pinning premium models; Copilot Free
uses Auto model selection.

| Agent | Model | Responsibility | Source |
| --- | --- | --- | --- |
| **Orchestrator** | Copilot default (Auto on Free) | Coordinates the specialists, breaks the work into phases, assigns explicit file scopes, manages dependencies, and validates the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Copilot default (Auto on Free) | Researches the repository and requirements, then creates the implementation plan, file assignments, dependencies, edge-case coverage, and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Copilot default (Auto on Free) | Defines the Project Pulse dashboard's user experience, information hierarchy, accessibility, responsive behavior, and visual styling. | `.github/agents/designer.agent.md` |
| **Coder** | Copilot default (Auto on Free) | Implements the assigned dashboard files, follows repository patterns, creates the runnable launch configuration when assigned, and validates the implementation. | `.github/agents/coder.agent.md` |

## How the team will build Project Pulse

1. The **Orchestrator** asks the **Planner** to research the repository and
   create a practical implementation plan for Project Pulse.
2. The **Orchestrator** gives the **Designer** ownership of the dashboard
   experience and styling, including readable project cards, status badges,
   priority treatment, responsive layout, and accessibility.
3. The **Orchestrator** assigns the **Coder** the implementation scope for the
   static dashboard, project data, and runnable VS Code launch configuration.
4. The **Orchestrator** integrates the specialist work, checks that the
   dashboard opens as Project Pulse rather than a directory listing, and
   reports the final validation and handoff.

The specialists do not stage, commit, or push changes; the learner controls
those Git operations after the team has completed its work.
