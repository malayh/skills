---
name: phase-orchestrator
description: Plan and implement one approved phase from an existing feature spec. Use for requests such as "plan phase N," "implement phase N," or continuing an approved phase; require plan approval before delegating implementation.
---

# Phase Orchestrator

Orchestrate one phase of an existing feature spec without writing implementation code or mutating Git.

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
2. Read the implementer and reviewer model and reasoning-effort values from the project's documented phase-orchestrator configuration block. When the harness exposes matching controls, select those values. Otherwise let each agent inherit the available settings and disclose that fallback.
3. Start exactly one implementation agent. Give it no inherited chat context when the harness supports that control. Its task must include the spec path, phase number, approved plan, and the instruction: "Follow the spec's Conventions section." Require it to implement only the phase, preserve unrelated changes, run applicable checks, and report changed files and results.
4. After the implementer finishes, start a separate read-only reviewer with no inherited chat context when supported. Give it the same phase context and baseline. Require it to review the current phase diff against the phase deliverables and Verify criteria and report only actionable defects with `file:line` references.
5. Evaluate the findings. Route real defects back to the original implementer when possible; otherwise use one replacement implementer. Never run multiple implementation agents concurrently. Allow at most two fix-and-review rounds after the initial review.
6. Stop earlier when the same blocker repeats or resolution needs user authority. After two fix-and-review rounds, stop and report remaining defects without marking the phase done.
7. Run the phase's Verify criteria and the project's lint and test equivalents from the applicable `AGENTS.md`. Never mark the phase done while any required check fails.
8. Make only the spec edits expressly required by its `Keeping this spec current` block: this phase's status and checklist, explicit deviation notes, surprising post-phase details, and unresolved problems in Open Decisions or a Follow-up note. Preserve rejected or changed text exactly as those rules require.

## Git Boundary

Never create worktrees or branches, stage, commit, merge, push, reset, stash, or otherwise mutate Git. Read-only status, diff, and log checks are allowed only to establish the baseline, review the phase diff, and detect contamination.

## Completion

Summarize the phase results, verification results, review outcome, and changed files. Ask the user to inspect the diff, run `/compact` when their interface supports it, and commit the phase. Never perform the commit.

Use conceptual capabilities rather than assuming specific tool names. Harness controls such as explicit model selection, zero-context agents, Plan mode, and `/compact` are conditional conveniences, not workflow requirements.
