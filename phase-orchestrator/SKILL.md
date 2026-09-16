---
name: phase-orchestrator
description: Plan and implement one approved phase from an existing feature spec. Use for requests such as "plan phase N," "implement phase N," or continuing an approved phase; require plan approval before implementation.
---

# Phase Orchestrator

Orchestrate one phase of an existing feature spec, implementing routine work directly and delegating only when the scope or risk warrants it, without mutating Git.

## Plan

1. Resolve and read the referenced spec and every applicable project `AGENTS.md` file.
2. Identify the requested phase, its current status, dependencies, deliverables, Verify criteria, Conventions, and spec upkeep rules. Stop if the spec lacks a well-formed numbered phase matching the request, its Conventions or Verify criteria are missing or unusable, dependencies are incomplete, or the phase is already complete unless the user explicitly changes scope.
3. Inspect the relevant code and use read-only Git status and diff checks to establish the baseline. Preserve pre-existing user changes. Stop before implementation if they overlap the phase or make attribution unsafe.
4. Produce a decision-complete plan covering files, implementation approach, and verification. State assumptions and any deviations from the spec.
5. Make no edits, spawn no subagent, and stop for explicit user approval of that plan.

Do not require or switch a product Plan mode. It may be used as an optional interface convenience when available.

## Implement

Proceed only when the user has explicitly approved the current phase plan.

1. Re-read the phase and relevant spec rules, then recheck dependencies and the baseline for contamination. Stop if the approved plan is stale or attribution is no longer safe.
2. Implement directly in the main session when the planned work is localized, familiar, and easy to reverse; introduces no dependency; and touches no security or authorization boundary, schema or migration, concurrency behavior, public or shared contract, or data-loss-sensitive path. Apply Ponytail at full intensity, follow the approved plan and spec Conventions, preserve unrelated changes, and run applicable checks.
3. Otherwise, delegate to exactly one implementation agent. Read the project's model policy and choose the cheapest available model and lowest reasoning effort likely to complete the work reliably. Treat configured models as defaults unless the project marks them as required. Escalate for risk, unfamiliar architecture, or a failed cheaper attempt.
4. Give a delegated agent no inherited chat context when the harness supports that control. Include the spec path, phase number, approved plan, and the instructions: "Follow the spec's Conventions section" and "Apply the Ponytail skill at full intensity." Point it only to relevant design sections, the requested phase, Conventions, and upkeep rules. Require it to implement only the phase, preserve unrelated changes, run applicable checks, and report only changed files, check results, and deviations.
5. Inspect the diff and implementation results. Resolve failed applicable checks before considering review.
6. Skip independent review when all of these are true: the diff is localized and easy to reverse; it changes no security or authorization boundary, schema or migration, concurrency behavior, public or shared contract, dependency, or data-loss-sensitive path; it follows the approved plan without material deviation; and applicable checks pass. State why review was skipped.
7. Otherwise, start one separate read-only reviewer with no inherited chat context when supported. Select its model independently using the same cheapest-capable policy. Give it the same focused phase context and baseline, and instruct it to apply Ponytail so it does not request speculative abstractions or unrelated cleanup. Require only phase-blocking defects in the format `file:line - defect - violated deliverable or Verify criterion`.
8. Evaluate the findings. If implementation was direct, fix confirmed defects directly; otherwise send them to the original implementer in one batch when possible or use one replacement implementer. Never run multiple implementation agents concurrently. Do not automatically review the fixes again. Re-review once only when a fix materially changes behavior or touches a high-risk area. Stop and report remaining defects if that review still fails or resolution needs user authority.
9. Run the phase's Verify criteria and the project's lint and test equivalents from the applicable `AGENTS.md`. Never mark the phase done while any required check fails.
10. Make only the spec edits expressly required by its `Keeping this spec current` block: this phase's status and checklist, explicit deviation notes, surprising post-phase details, and unresolved problems in Open Decisions or a Follow-up note. Preserve rejected or changed text exactly as those rules require.

## Git Boundary

Never create worktrees or branches, stage, commit, merge, push, reset, stash, or otherwise mutate Git. Read-only status, diff, and log checks are allowed only to establish the baseline, review the phase diff, and detect contamination.

## Completion

Summarize the phase results, verification results, review outcome or skip reason, and changed files. Ask the user to inspect the diff, run `/compact` when their interface supports it, and commit the phase. Never perform the commit.

Use conceptual capabilities rather than assuming specific tool names. Harness controls such as explicit model selection, zero-context agents, Plan mode, and `/compact` are conditional conveniences, not workflow requirements.
