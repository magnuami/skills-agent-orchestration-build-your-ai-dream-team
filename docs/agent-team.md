# Agent team

For Mona's Project Pulse dashboard, I am using a small specialist team defined in `.github/agents/` and orchestrated from GitHub Copilot CLI inside a Codespace.

## Team members

### Orchestrator
- Model: Claude Opus 5.5 (copilot)
- Responsibility: Coordinates the overall workflow, breaks the work into phases, delegates to specialist agents, and verifies that design and implementation fit together.
- Definition: `.github/agents/orchestrator.agent.md`

### Planner
- Model: Claude Opus 5.5 (copilot)
- Responsibility: Researches the repo, identifies dependencies and edge cases, and creates the practical implementation plan before coding begins.
- Definition: `.github/agents/planner.agent.md`

### Designer
- Model: Gemini 3.7 Flash (copilot)
- Responsibility: Focuses on UI/UX, visual hierarchy, accessibility, spacing, and the polished look of the Project Pulse dashboard.
- Definition: `.github/agents/designer.agent.md`

### Coder
- Model: GPT-5.5 (copilot)
- Responsibility: Implements the static dashboard files, creates supporting configuration such as `.vscode/launch.json`, and validates the runnable result.
- Definition: `.github/agents/coder.agent.md`

## How the team works together

The workflow is driven by GitHub Copilot CLI in a Codespace: the Orchestrator first asks the Planner for a plan, then delegates design work to the Designer and implementation work to the Coder. This keeps the work staged, shared by responsibility, and easier to validate before finishing the Project Pulse dashboard.
