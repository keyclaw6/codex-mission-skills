# Codex mission skills

Skills for GPT-5.6 Sol on Codex. They counter three failure modes in long
agentic coding runs: over-engineering, losing the mission as context
compacts, and stopping subagents before they finish.

Runtime mission loop:

    human ↔ sentinel → separate orchestrator thread → native children
                           └→ independent review-elegance verifier

- `sentinel` — optional detached supervisor: launches the orchestrator,
  wakes on a scheduled heartbeat, compares progress with the named mission
  record, and accepts or re-briefs the result.
- `orchestrate-mission` — execution, integration, and evidence owner;
  returns a completion candidate to its caller.
- `review-elegance` — bounded, read-only independent verification of one
  named implementation state.

Optional external handoff:

- `prepare-pro-review` — explicitly invoked publishing and review-pack
  preparation for separate ChatGPT GPT-5.6 Sol Pro sessions. Not part of
  ordinary mission completion.

Each skill installs as `<skills-dir>/<name>/SKILL.md`. Projects using these
skills follow [agent-first-project-template](https://github.com/keyclaw6/agent-first-project-template):
human-owned `VISION.md`, OpenSpec changes carrying a definition of done,
and `npm run check` as canonical repository verification.
