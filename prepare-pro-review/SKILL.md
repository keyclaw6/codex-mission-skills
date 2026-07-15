---
name: prepare-pro-review
description: "Publish a local coding project to remote main and create a comprehensive pack of two or three distinct, bounded review prompts for separate ChatGPT GPT-5.6 Sol Pro sessions. Use when the user explicitly wants a general audit, architecture review, implementation or change review, security review, forward-looking feature or architecture recommendations, roadmap review, or another custom repository review, with the exact repository URL and commit recorded in prompt files under Downloads. Make every reviewer seek the best solution, using elegance, YAGNI, and minimalism as design principles without optimizing for patch size, least effort, or speculative future needs."
---

# Prepare Pro Review

Publish one exact repository state, then prepare independent review assignments that the user can run concurrently in fresh ChatGPT Pro chats. Make the prompts different but the pack collectively comprehensive for the selected review mode. Do not perform the requested reviews yourself unless the user separately asks for that.

Invocation authorizes committing the current repository state, merging the active work into `main`, pushing `main`, and writing the handoff folder under `~/Downloads`. Keep that authority limited to the repository and branches in scope.

## Core review principle: Elegance and YAGNI

Seek the best solution for the actual requirement across every mode and every generated prompt. Use elegance, YAGNI (You Aren't Gonna Need It), and minimalism as design principles, not as reasons to prefer a limited patch.

- Require a present requirement, demonstrated failure mode, or repository evidence for every proposed change.
- Optimize for the best overall result: correctness, security, robustness, conceptual coherence, maintainability, performance, operational fit, and long-term cost within the real scope.
- Make elegance a primary criterion. Favor clear concepts, coherent boundaries, direct data and control flow, strong invariants, and minimal incidental complexity.
- Treat minimalism as economy of concepts, dependencies, moving parts, and ongoing obligations. Do not equate minimalism with fewer changed lines, a narrow diff, or the least implementation effort.
- Allow and recommend a substantial refactor or redesign when it produces a materially better, more elegant result for the current requirement.
- Treat robustness as handling credible current failures with clear invariants and validation, not as adding layers for hypothetical futures.
- Challenge premature abstractions, generic frameworks, extension systems, compatibility layers, configuration knobs, new services, dependencies, caches, queues, observability machinery, and feature ideas that lack a concrete current use case.
- Prefer adopting a proven existing capability over building infrastructure when it produces the better design and satisfies the actual requirement.
- Do not use YAGNI to dismiss a credible correctness, security, privacy, recovery, or data-loss risk; addressing such a risk is a present need.
- Compare credible alternatives and recommend the best one. Explain why it is superior for the current requirement, what tradeoffs it carries, and how elegance, YAGNI, and minimalism informed the judgment.
- Defer a proposal or place it under `Do not pursue now` when it lacks a current justification, not merely because it would require meaningful change.

## Required outcome

Produce all of the following or report the exact blocker:

- A clean, committed review target on `main`.
- A successful push whose remote `main` SHA equals the local `main` SHA.
- Two prompts for a narrow review or three prompts for a broad review. Never create one omnibus prompt or more than three.
- A review mode, target, desired outcome, and constraints derived from the user's invocation and repeated in every prompt.
- An explicit elegance, YAGNI, and minimalism instruction in every prompt, with a best-solution justification for every proposed fix, redesign, dependency, or feature.
- Distinct primary ownership for every important concern in the selected review mode, with no material coverage gap across the pack.
- A folder named `~/Downloads/<repo>-gpt-5.6-pro-review-<YYYYMMDD-HHMMSS>/` unless the user supplies a run name.
- One Markdown file per prompt, named `01-<scope>.md`, `02-<scope>.md`, and optionally `03-<scope>.md`.
- The sanitized HTTPS repository URL, branch `main`, full commit SHA, and exact commit URL in every prompt.

Generate the prompt files only after the remote SHA has been verified.

## Invocation examples

Accept ordinary language after the explicit skill name:

```text
$prepare-pro-review Run a general audit of the whole repository and verify that its important workflows actually work.
$prepare-pro-review Review the whole architecture, its boundaries, and whether a simpler target architecture would be better.
$prepare-pro-review Review the implementation changes since origin/main for correctness, maintainability, and integration risk.
$prepare-pro-review Recommend worthwhile features and architecture changes for the next stage of this project. Be YAGNI-lean.
$prepare-pro-review Perform a security review of authentication, authorization, and tenant isolation.
```

## 1. Establish the review target

1. Find the repository root and read its applicable instructions.
2. Inspect `git status`, the current branch, remotes, recent commits, diffs, and relevant project documentation.
3. Convert the user's request into a review brief with four explicit fields:
   - **Mode:** `general-audit`, `architecture`, `implementation`, `recommendations`, `security`, or `custom`.
   - **Target:** whole repository, current change, named subsystem, named workflow, or another concrete boundary.
   - **Desired outcome:** defects, architectural judgment, validation of behavior, proposed improvements, prioritized recommendations, or another stated result.
   - **Constraints:** always include elegance, YAGNI, and minimalism, plus priorities such as compatibility, security, performance, delivery horizon, or areas to exclude.
4. Preserve the user's explicit mode and wording. When no mode is given, default to `general-audit` and state that assumption in the handoff. Ask only if different interpretations would materially change the review.
5. Record the starting branch and a comparison base before merging:
   - On a work branch, prefer its merge base with remote `main`.
   - On `main`, prefer the pre-push remote `main` SHA.
   - Omit a comparison base for an explicitly whole-repository architecture review when it would distract from the task.
6. Determine the most relevant verification command from repository instructions. Run it and record the command and result for the prompts. A failing check is review context; do not describe it as passing.

## 2. Commit, merge, and push

Inspect every dirty path before publishing. Include all current repository changes in the review state; never silently leave part of the working tree behind. Stop instead of publishing if a path appears to contain credentials, private keys, tokens, personal data, or another obvious secret.

Use ordinary, non-rewriting Git operations:

1. Fetch the remote state without pruning user branches.
2. Commit uncommitted changes with a concise message that describes the actual change. Do not create an empty commit.
3. Bring local `main` together with remote `main` without force-pushing, resetting, rebasing shared history, or discarding work.
4. If the starting branch is not `main`, merge it into `main`. Also merge only additional branches the user explicitly named. Do not sweep every local or remote branch merely because it exists.
5. Resolve a conflict only when the intended resolution is evident from the code and instructions. Otherwise stop and show the conflict.
6. Run the selected verification on the resulting `main`. Record failures faithfully; the user's explicit workflow still permits publishing a failing review target unless publication would expose secrets or leave unresolved conflicts.
7. Push local `main` directly to remote `main`. Do not substitute a pull request or another branch.
8. Compare `git rev-parse main` with the SHA returned for remote `refs/heads/main`. Continue only when they match.

If authentication, branch protection, missing `main`, divergent history requiring a destructive operation, or another remote rule prevents the push, stop. Do not generate prompts that imply the remote contains an unverified state.

## 3. Build stable source links

Select `origin` when present; otherwise use the sole relevant remote. Convert SSH remotes to a credential-free HTTPS repository URL and remove a trailing `.git`. Never place credentials or authenticated URLs in a prompt.

Create an exact commit URL when the host supports it, for example:

- GitHub: `<repo-url>/commit/<sha>`
- GitLab: `<repo-url>/-/commit/<sha>`

Check whether the repository and exact commit are readable without authentication. Treat public-web access as best effort: ChatGPT search does not guarantee that every public URL can be fetched.

Standard ChatGPT Pro models cannot be assumed to have ChatGPT Apps, including a GitHub connector. If anonymous access cannot be confirmed:

1. Keep the repository URL and commit URL in every prompt.
2. Create one UTF-8 text source bundle per prompt from the exact committed tree, not from a later working tree.
3. Include only files needed for that prompt's assigned scope. Use clear `===== path/to/file =====` delimiters and include the relevant diff or change summary when applicable.
4. Exclude secrets, binaries, vendored dependencies, dependency caches, and generated build output. Do not claim omitted content was reviewed.
5. Keep each bundle comfortably below ChatGPT's file limits; prefer one file under 6 MB. Split into no more than three parts for a prompt when necessary.
6. Name bundles `01-source-context.txt`, `02-source-context.txt`, and so on, and make the matching prompt explicitly tell the user which file or files to attach.

## 4. Partition the review

Treat comprehensiveness as a pack-level requirement. Each prompt must answer a different primary question, while the combined prompts cover the entire selected mode and target.

Before writing prompts, create a small internal coverage map that assigns each required concern to one primary prompt. Permit secondary overlap only for cross-cutting evidence. Check the map for gaps and unnecessary duplication.

Use three prompts for a whole-repository or otherwise broad review. Use two only when the target is narrow enough that three would manufacture work.

### General audit

Cover whether the system is sound and its important workflows work:

1. **Architecture and system coherence:** boundaries, dependency direction, data ownership and flow, contracts, configuration, and YAGNI simplicity.
2. **Implementation and runtime behavior:** correctness, security, concurrency, data integrity, failure handling, performance, and important end-to-end paths.
3. **Verification and operational readiness:** test coverage and quality, migrations, integrations, deployment, recovery, operability, and gaps between claims and observed behavior.

### Architecture review

Keep all prompts architectural, but give them different architectural lenses:

1. **Structure and boundaries:** topology, module and service responsibilities, dependency direction, APIs, data ownership, and coupling.
2. **Runtime qualities:** scalability, reliability, security boundaries, concurrency, consistency, performance, deployment, and failure recovery as consequences of the architecture.
3. **Simplicity and evolution:** YAGNI, accidental complexity, adopt-versus-build choices, extensibility actually justified by requirements, alternative target structures, and migration cost.

### Implementation or change review

For a narrow change, use the first two prompts; add the third for a broad implementation:

1. **Correctness and safety:** real control and data flow, edge cases, validation, security, data integrity, concurrency, and error handling.
2. **Code and contract quality:** APIs, invariants, maintainability, dependency use, performance, compatibility, and fit with the surrounding design.
3. **Integration and verification:** tests, migrations, configuration, external integrations, release behavior, and end-to-end validation.

### Recommendations review

Review the future of the project rather than disguising suggestions as defects:

1. **Current-state gaps and opportunities:** user or operator needs, missing capabilities, friction, technical constraints, and evidence that a change is worthwhile.
2. **Feature and architecture options:** concrete alternatives, reuse or adoption opportunities, build-versus-buy tradeoffs, architectural implications, risks, and options that should be rejected.
3. **Prioritized direction:** ranked recommendations, expected value, effort, dependencies, sequencing, migration path, validation plan, and explicit reasons to defer lower-value ideas.

### Security review

1. **Threat model and boundaries:** assets, actors, entry points, trust boundaries, authentication, authorization, isolation, and abuse cases.
2. **Implementation vulnerabilities:** input handling, injection, secrets, cryptography, dependencies, data exposure, concurrency, and unsafe failure behavior.
3. **Defense and verification:** security tests, supply chain, configuration, monitoring, incident response, recovery, and residual risks.

### Custom review

Derive two or three orthogonal questions from the requested outcome. State why the partition is collectively sufficient. Do not silently turn a recommendation request into a defect audit or an architecture request into an implementation review.

Adapt every lens to the actual repository. Give each assignment a concrete subsystem, path set, workflow, or concern boundary while allowing it to inspect surrounding code needed to validate interactions.

## 5. Write each prompt

Keep each prompt concise and self-contained. Use this contract:

```markdown
# Independent review <N>: <specific title>

Run this as a bounded, read-only review in GPT-5.6 Sol Pro.

Repository: <credential-free HTTPS URL>
Branch: main
Exact commit: <full SHA>
Commit URL: <exact URL>
Comparison base: <SHA or "whole repository review">
Compare URL: <URL when available>
Review mode: <general-audit | architecture | implementation | recommendations | security | custom>
Review target: <whole repository, change, subsystem, or workflow>
Review objective: <the user's actual goal>
Review-pack role: <N of total, with a one-line summary of every prompt's ownership>
Review principle: Seek the best solution. Make elegance the key criterion, apply YAGNI, preserve minimalism, and do not optimize for patch size or least effort.
Verification already run: <command and truthful result>

## Your assigned scope
<specific paths, subsystem, and concerns>

## Deliberately out of scope
<the concerns assigned to the other sessions>

## Source access
<Tell the reviewer to open the exact public commit, or tell the user which source-context files to attach. Require an immediate access check.>

## Task
Inspect enough surrounding code to validate interactions, but spend the review budget on the assigned scope. Trace real control flow and data flow. Validate every finding, judgment, or recommendation against the exact commit. Be comprehensive inside the assigned lens, not across the other prompts' lenses.

Seek the best solution throughout. For every proposed fix, redesign, dependency, abstraction, or feature, state the current evidence or use case, compare credible alternatives, and explain why the recommendation is the best overall choice. Make elegance, YAGNI, and minimalism explicit parts of the judgment. Minimalism means conceptual economy and absence of unjustified machinery; it does not mean fewer changed lines or less effort. Recommend a substantial change when it materially improves the result. Robustness must address credible current failure modes without becoming speculative future-proofing. Explicitly reject or defer unjustified additions.

Do not modify the repository. Do not give generic best practices, style nits, or unsupported redesigns. Do not broaden into work assigned to the other sessions. If source access is incomplete, state exactly what is missing and do not pretend to have reviewed it.

## Output
<Insert the mode-specific output contract. Require evidence and permanent links pinned to the exact commit when the host permits them.>
```

Replace every placeholder. Do not tell a reviewer to coordinate with another chat; the sessions are independent and concurrent. Do not ask any one session to synthesize the other reviews.

Choose the output contract by mode:

- **General audit, implementation, or security:** require a concise verdict; at most 8 actionable findings ordered `P0`–`P3`; exact code evidence and impact; the best proposed fix with alternatives and tradeoffs; validation gaps; and an explicit no-findings statement when appropriate.
- **Architecture:** require an architecture verdict; evidenced structural strengths and liabilities; violated or well-served quality attributes; elegant alternatives with tradeoffs; the recommended best direction; prioritized changes; migration implications; and validation gaps. Use severity only for actual architectural risks, not preferences, and reject extensibility without a present use case.
- **Recommendations:** require a current-state assessment and at most 8 ranked recommendations. For each include repository evidence, present problem or opportunity, expected value, the recommended best change, credible alternatives, effort, risk, dependencies, migration or rollout, and how to validate value. Require a `Do not pursue now` section for ideas that fail YAGNI.
- **Custom:** derive an output contract that directly answers the requested decision, with evidence, best-solution reasoning, elegance and YAGNI analysis, prioritization, uncertainty, and next validation steps.

## 6. Verify and hand off

Before finishing:

1. Confirm there are exactly two or three prompt files.
2. Search every prompt for the repository URL, `main`, and the full SHA.
3. Confirm every prompt states the same review mode, target, objective, and pack size.
4. Confirm the coverage map has no material gap for the selected mode and target.
5. Confirm every prompt explicitly requires the best solution and names elegance, YAGNI, and minimalism as review principles.
6. Confirm scopes and expected outputs are materially distinct and all placeholders are gone.
7. Confirm private-source bundle names in prompts exactly match files in the folder.
8. Print the selected review brief, coverage summary, final folder path, and filenames.
9. Tell the user to select GPT-5.6 Sol Pro, open one fresh ChatGPT chat per prompt, attach the named source bundle when present, and launch the chats concurrently.

Do not claim that ChatGPT can access the repository merely because a URL is present. Distinguish verified remote publication from ChatGPT's best-effort web retrieval.
