# Codex mission skills

Skills for GPT-5.6 Sol on Codex. They counter three failure modes in long
agentic coding runs: over-engineering, losing the mission as context
compacts, and stopping subagents before they finish.

- `sentinel` — takes a mission from the user, launches an orchestrator in a
  separate thread, checks in on a schedule, corrects course, verifies
  completion.
- `orchestrate-mission` — parent-led multi-subagent execution: spec-first
  planning, repo-native state, elegance as the acceptance bar, patience
  with running children.
- `review-elegance` — bounded independent review: does it actually work,
  and what can be deleted.
- `prepare-pro-review` — publishes the repository and prepares bounded
  review prompts for separate ChatGPT GPT-5.6 Sol Pro sessions.

Each skill installs as `<skills-dir>/<name>/SKILL.md`. Projects using these
skills follow [agent-first-project-template](https://github.com/keyclaw6/agent-first-project-template):
human-owned `VISION.md`, OpenSpec changes with a definition of done, and
`npm run check` as canonical verification.
