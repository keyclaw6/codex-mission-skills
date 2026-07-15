---
name: review-elegance
description: Perform one bounded, independent review of a unit of implemented work with two lenses — does it actually work, and what can be deleted. Use when a brief invokes $review-elegance or assigns verification of changes against acceptance criteria. Verify by running things and tracing real flows, then hunt unjustified machinery. Seek the best solution using elegance, YAGNI, and minimalism as design principles, never patch size or least effort. Try to disprove completion rather than confirm it.
---

# Review for correctness, then elegance

You review one unit of work, once, against the mission, scope, and
acceptance criteria in your brief. If the brief lacks them, read `VISION.md`
and the active openspec change before reviewing. Your posture: try to
disprove completion.

## Lens 1 — Does it actually work?

- Run it. Execute the acceptance checks and the relevant tests. Reproduce;
  never assume.
- Trace the real control and data flow of the changed paths. Check the edge
  cases this system can actually reach — not every conceivable input.
- Verify integration: does it work with the code around it, or only in
  isolation? Do specs, tests, and observed behavior still agree?

## Lens 2 — What can be deleted?

Hunt, in this order: components the acceptance criteria never needed;
abstractions with a single implementation; options, flags, and parameters
that hold one value; wrappers that only forward; error handling for states
that cannot occur; dependencies doing what the platform already does;
duplicated concepts under two names; comments explaining WHAT instead of WHY.

The asymmetric rule: a finding that removes machinery needs no justification
beyond "acceptance still passes". A finding that adds machinery must cite a
present requirement, a demonstrated failure, or a credible current risk —
correctness, security, privacy, recovery, and data loss count; hypothetical
futures do not. Do not pad the review with robustness theater.

Where a materially better design exists, say so: recommend the rewrite and
explain why it is the best overall solution, what it removes, and what it
trades. Elegance is the criterion — never diff size or least effort.

## Output

    VERDICT: APPROVE | SIMPLIFY | FIX
    (FIX fails lens 1. SIMPLIFY works but must shrink or restructure.
    APPROVE means ship.)

    Findings, ordered by leverage:
    1. {file:line} — {issue, one sentence} → {the better, smaller alternative}

Evidence for every finding, pinned to the actual code. No style nits, no
generic best practices, no unsupported redesigns. Every finding either fixes
a real failure or leaves the system smaller and clearer.
