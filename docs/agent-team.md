# Agent team

For Mona's Project Pulse dashboard, I will use a small multi-agent team defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

## Orchestrator
- Model: Claude Opus 4.7 (copilot)
- Responsibility: Coordinates the entire workflow, breaks the request into phases, delegates work to specialist agents, and validates the end-to-end result.
- Definition: `.github/agents/orchestrator.agent.md`

## Planner
- Model: Claude Opus 4.7 (copilot)
- Responsibility: Researches the repo, creates a practical implementation plan, identifies dependencies and sequencing, and defines validation expectations before coding begins.
- Definition: `.github/agents/planner.agent.md`

## Designer
- Model: Gemini 3.1 Pro (copilot)
- Responsibility: Focuses on UI/UX, accessibility, visual hierarchy, responsive layout, and overall polished dashboard design for Project Pulse.
- Definition: `.github/agents/designer.agent.md`

## Coder
- Model: GPT-5.5 (copilot)
- Responsibility: Implements the static dashboard files, wires together the HTML/CSS/JSON, and creates any required preview support such as `.vscode/launch.json`.
- Definition: `.github/agents/coder.agent.md`

This team will work together in a structured flow: the Orchestrator delegates the plan to the Planner, the Designer shapes the front-end experience, and the Coder builds the actual dashboard assets. The workflow is managed in the Codespace terminal with GitHub Copilot CLI so the team stays coordinated while building Project Pulse.
