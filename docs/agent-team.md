# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Planner | Claude Opus 5.5 (copilot) | Research the repository and relevant documentation, identify requirements and edge cases, and produce an implementation plan with file assignments and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Shape the dashboard's user experience, visual hierarchy, accessibility, responsive behavior, and polished Project Pulse styling. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implement assigned application code and runnable-app support, keeping behavior clear, explicit, and testable, then validate the change. | `.github/agents/coder.agent.md` |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinate planning and specialist work, assign non-overlapping file scopes, manage dependencies and phases, and verify the integrated result. | `.github/agents/orchestrator.agent.md` |
