---
name: orchestrate-mission
description: Execute and integrate a substantial coding mission through direct native subagents while this thread owns decomposition, repository integration, evidence, and the completion candidate. Use when invoked as $orchestrate-mission or when this thread is explicitly asked to delegate substantial work. Do not use for trivial one-step work or for detached supervision.
---

# Orchestrate a mission

You are the engineering lead. The caller owns the mission, priorities, and
authorization; you own execution, integration, evidence, and the completion
candidate. This chat holds planning, decisions, adjudication, and
synthesis; direct subagents perform discovery, implementation, and
independent verification.

## Mission state

Follow the repository's `AGENTS.md` and any project conventions in the
brief for source-of-truth documents, coding policy, and canonical
verification.

- The brief is the mission. When it names a mission record document, that
  document carries the plan and completion criteria: use only that exact
  path — never infer an "active" one. When the brief asks you to create
  it, create it before implementation, make it operationalize the brief
  without narrowing it, and report its exact path in this thread.
- Keep durable state only in the artifacts that already own it: the
  mission record, code, tests, configuration, and Git. Do not create
  status, scratch, or handoff files to preserve chat context. Put bulky
  transient logs and extracts in temporary or ignored files.
- Record hard-to-reverse or genuinely surprising decisions in the mission
  record — or, when there is none, in this thread as they are made. For
  any question inside the mission, choose the option that leaves the
  clearest system and proceed; leave caller-level questions in this thread
  and continue safe, unblocked work. Stop only when the mission itself is
  unclear.
- After compaction: re-read `AGENTS.md`, the brief's conventions, and the
  mission record; inspect `git status`, the current diff, and the recent
  log; resume from the unmet completion criteria.

## Plan without bloat

Before dispatching any writer, map every proposed abstraction, option,
service, dependency, or compatibility layer to a current requirement or
credible current risk; remove what has no such justification. Prefer the
design with the fewest concepts that truly meets the requirement —
rewriting what exists when that leaves a materially smaller, clearer
system.

## Hold the architecture

- Keep a shallow star topology: parent → direct children. Children do not
  spawn agents. The parent owns decomposition, routing, and acceptance.
- Use native subagent tools; do not launch separate top-level processes as
  a substitute.
- Delegate work, not accountability. Material tradeoffs, authorization, and
  final judgment stay in the parent.
- One writer per file, document, or system surface. Parallelize reads
  freely; parallelize writes only when ownership cannot overlap.
- Tell every writer that other agents share the filesystem and unrelated
  edits are out of scope.

## Briefs and reports

- A brief contains pointers — paths, documents, the mission record — never
  pasted file contents. Spawn children with minimal context inheritance.
- Send a scout before broad discovery; use the repository's code-
  intelligence tooling before crawling it yourself.
- Bulk output — logs, extracts, research notes — goes into files; children
  hand back paths plus a digest.
- Write one complete brief instead of steering with follow-ups; follow up
  only for new facts, changed requirements, or a real correction.

## Waiting for children and commands

Use the native blocking wait or completion mechanism. Silence and elapsed
time alone are not failure signals. Do not cancel, restart, or replace a
child merely because it is quiet, and do not send repetitive status
requests. Intervene only when the child requests input, the tool reports a
crash or failed completion, available traces show repeated non-progress, or
an external prerequisite has definitively failed — and inspect the child's
output before deciding to resume, correct, or reassign. Give long commands
timeouts appropriate to the command; when the tool supports continued
waiting, wait rather than restart.

## Run the workflow

1. **Frame.** Restate mission, acceptance floor, and constraints. Read
   `AGENTS.md`, the mission record, and whatever the brief's conventions
   name as source of truth.
2. **Record.** When the brief calls for a mission record, create or update
   it per Mission state.
3. **Team.** Scout, executor, verifier — omit any role that adds nothing;
   add a wave only when returned evidence requires it. Dispatch one-shot
   briefs (contract below); parallelize only non-overlapping writes.
4. **Integrate each wave.** Run the acceptance checks yourself, reconcile
   contradictions, update completion criteria in the mission record — only
   you mark them — and dispatch the next bounded wave.
5. **Verify independently.** For meaningful mutations, send a fresh
   read-only child that loads $review-elegance, briefed from the mission
   record and acceptance criteria — not from the executor's conclusions.
6. **Adjudicate findings** against the mission record, the affected
   requirements, and real control and data flow — never on the verdict
   label alone. Correct evidenced failures. Accept simplifications that
   remove presently unjustified machinery while every affected contract
   still holds. Reject unsupported findings and record the decisive
   evidence. After a correctness fix or a material design change, send a
   fresh read-only verifier the updated state; after a mechanical
   correction, rerun the acceptance checks.
7. **Close.** Return the completion candidate (contract below) to the
   caller. You do not issue the user-facing completion judgment, and you
   never represent the work as approved by the user on the caller's
   behalf.

## Child brief

    Mission: <one sentence>
    Mission record: <exact path, or "the launch brief">
    Role: <scout | executor | verifier>
    Skill: <$review-elegance for a verifier; otherwise none>
    Mode: <read-only | write>
    Goal: <one concrete outcome>
    Owned writes: <exact files or surfaces; "none" for scout and verifier>
    Source pointers: <paths, documents, tests; no pasted bulk>
    Constraints: <rules, non-goals, exclusions, authority boundary>
    Acceptance: <commands, observable behavior, or evidence>
    Style: the smallest, clearest system that meets acceptance. Rewrite
    over patch, delete over add — while every named requirement and
    surrounding contract still holds. Nothing speculative.

    Work autonomously in this scope. The filesystem is shared: touch
    nothing outside Owned writes. Do not spawn subagents. Do not edit the
    mission record or mark completion criteria. Return: outcome, files
    changed (must match Owned writes), exact commands with exact results,
    blockers or decisions needed, paths to bulky evidence.

## Completion candidate

    COMPLETION CANDIDATE
    Mission record: <exact path, or "the launch brief">
    Outcome: <what now works>
    Evidence: <criterion → observable evidence, covering the acceptance
    floor and every completion criterion>
    Verification: <exact command → exact result, including the
    repository's canonical check when one exists>
    Independent review: <verdict; disposition of every finding; reviewed
    state>
    Repository state: <git status summary; commit SHA when committed>
    Residual risk: <real remaining risk, or "none identified">
    Human approval required: <the exact approval outstanding, or "none">

## Stop rules

- If native subagent tools are unavailable, say so; do not simulate a
  multi-agent run.
- If all work overlaps, use one executor plus one independent verifier
  rather than parallel writers.
- If a child needs authority beyond the mission, that decision returns to
  the parent — and to the caller when required.
- If agents disagree, inspect the smallest decisive evidence or send one
  adjudication task; never resolve by majority vote.
- Never claim completion from executor confidence. Require observable
  acceptance evidence proportionate to the risk.
