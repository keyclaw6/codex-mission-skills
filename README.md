# Codex mission skills

Skills for GPT-5.6 Sol on Codex. They counter four failure modes in long
agentic coding runs: over-engineering, losing the mission as context
compacts, stopping subagents before they finish, and mistaking activity for
evidence of progress.

Runtime mission loop:

    human ↔ meta-agent → separate orchestrator thread → native children
                             └→ independent review-elegance verifier

- `meta-agent` — optional detached mission controller: approves the plan,
  enforces milestone, liveness, time, cost, and evidence gates, recovers
  stalls, and independently accepts or re-briefs the result.
- `orchestrate-mission` — execution, integration, and evidence owner;
  returns a completion candidate to its caller.
- `review-elegance` — bounded, read-only independent verification of one
  named implementation state.

Optional external handoff:

- `prepare-pro-review` — explicitly invoked publishing and review-pack
  preparation for separate ChatGPT GPT-5.6 Sol Pro sessions. Not part of
  ordinary mission completion.

The runtime skills are convention-agnostic. State a project's conventions
in the mission prompt — source-of-truth documents, spec system, canonical
checks — and the Meta Agent passes them through verbatim; the mission record
generalizes to whatever repository document carries the plan and completion
criteria, or the launch brief itself. Each skill installs as
`<skills-dir>/<name>/SKILL.md`.
