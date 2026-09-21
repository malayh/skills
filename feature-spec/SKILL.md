---
name: feature-spec
description: Produce a build-ready feature spec by exploring the relevant code, resolving design-changing ambiguities, and defining independently shippable implementation phases. Use this whenever the user wants a spec, design doc, technical plan, RFC, or implementation plan written to a file, and when a feature is large enough to need phasing even if they never say "spec". Triggers include "write a spec for X", "spec this out", "plan the implementation", "break this into phases", "design doc for", "how should we build X", or a substantial set of requirements needing a plan. Skip it for single-file bug fixes and changes small enough to implement directly.
---

# Feature Spec

Produce one artifact: a spec markdown file that a competent agent (or human) can build from without asking you anything else.

You do not implement. You do not update specs after the fact — the spec you write carries its own upkeep rules, and whoever builds from it follows those.

## The bar

A spec is good when a reader who has never seen the codebase can build the feature and arrive at the same design you would have. That takes four things, and only four:

1. **Every decision is stated flatly and is already made.** Not "we could either…". If it's genuinely still open, it goes in Open Decisions, not the body.
2. **Every non-obvious decision carries its why.** The why is the load-bearing part — it's what lets an implementer deviate correctly when reality differs from the spec. A why that just restates the decision ("we use a webhook because webhooks are reliable") is worse than none; delete it.
3. **Surprises are surfaced.** Anything that would make an implementer say "wait, what?" mid-build — a shared file that races, a sanitizer that lets `,` through, a binary whose output differs between builds. Finding these is most of the value of exploring the codebase. If you found nothing surprising, you didn't look hard enough.
4. **Phases are real boundaries.** Each one ships alone, leaves the system working, and can be verified without the next phase existing.

Everything else is packaging.

## Process

### 1. Explore

Explore directly with focused searches and file reads. Find what exists, what is reusable, what blocks the feature, and what would surprise an implementer.

Delegate read-only exploration only when the feature spans multiple repositories, contains genuinely independent subsystems, or would add substantial noise to the main context. Give each agent one bounded question and use no more than two exploration agents total. Do not delegate when targeted searches can establish the relevant flow.

Check other repositories only when the request or codebase provides evidence that the feature crosses repository boundaries. Ask if that boundary is unclear and materially changes scope.

### 2. Report findings

Before the first question, report only findings that affect scope, contracts, risks, or a user decision. Use at most 5-8 short bullets. Skip the report when there is nothing the user needs to confirm or correct.

### 3. Interrogate

Use the multiple-choice question tool for up to three material questions per round. One round is the default; continue only when an answer exposes another implementation-blocking decision. Each option gets a real tradeoff in its description. Put your recommended option first and mark it `(Recommended)`.

Ask about what actually changes the design:

- Ambiguities in the requirements with two or more defensible readings
- Scope boundaries — what's deliberately *not* in this
- Anything that constrains the data model or the contract between components
- Failure behavior: what happens when the external thing is down, slow, or lies
- Migration and backfill for anything already in production
- Anything where you'd otherwise be guessing at the user's business context

Don't ask what the codebase already answers or what would not change the spec. State a safe, reversible default instead of asking about a minor preference.

Stop questioning when remaining uncertainty would not materially change implementation. Park non-blocking uncertainty in Open Decisions with the default the implementer should use.

### 4. Pick the path, then write

Ask where the file goes unless the user already said. Look for an existing convention first (`design/`, `docs/`, `specs/`) and propose a path matching it.

Then think hard about phases before writing a line of the phase section. This is the part that most rewards slow thinking — see Phase design below.

Write the file. Don't summarize it back in chat; the file is the deliverable. Say where it is and flag anything you're unsure about.

### 5. Iterate

Revise when the user requests changes. Do not add a review agent or another exploration pass by default.

Before delivering, perform one inline scan for placeholders, contradictions, missing phase verification, and unresolved implementation blockers. Fix issues directly, then stop.

## Structure

Mandatory: **Goal**, **Scope (In/Out)**, **Decisions**, **Considered & rejected**, **Implementation phases**, **Open Decisions**, **Risks**.

Everything else is by need. Design sections between Decisions and phases are where most of the spec lives, and their shape follows the feature — data model, endpoints, state machine, the loop, the flow. Name them for what they are. Don't invent sections to fill a template, and don't drop a section the feature obviously needs because it isn't on the mandatory list.

Scale detail to risk and complexity. Use the fewest sections and phases that leave the design unambiguous and each phase independently shippable.

`Considered & rejected` can be its own section or inline where the decision is made — inline is often better, since the rejected option is most interesting next to the one that won. Either way it must exist somewhere.

Tables, ASCII diagrams, short code snippets, and file lists are all fair game — none are required. Use one when it genuinely beats bullets: a state machine as a diagram, endpoints as a table, the exact buggy line as a snippet. Reach for prose bullets by default.

## Writing rules

Short bullets. No filler. No hedging. No "we should consider" — decide, or park it in Open Decisions.

Write it the way you'd brief a peer who is competent and busy: assume they know the stack, don't explain what a webhook is, don't pad transitions. A line either changes what someone builds or it comes out.

Name real things — `services/auth.py:88`, `SignupSession.status`, `map.json`. Vague references ("the auth service") make a spec unbuildable.

## Phase design

A phase is a stopping point, not a work chunk. The test: if the project were cancelled the moment this phase landed, would the system be in a coherent state? If no, the boundary is wrong.

Phase one is usually whatever unblocks everything else — a contract, a schema, a bug that must be fixed before anything downstream is safe. Sequencing after that follows dependencies, not effort.

Record dependencies that constrain execution. Work is performed one requested phase at a time; do not suggest combining or concurrently executing phases. If implementation later needs to cross a phase boundary, it must explain why and get user approval.

Each phase carries:

- **Status marker** on the heading. `—` (not started) → `IN PROGRESS` → `CODE DONE, VERIFICATION PENDING` → `✅ DONE`. Add a qualifier when the truth needs one.
- **Dependencies** — `deps: P1, P2`.
- **Deliverables** — bullets naming files/functions and the approach. What and how.
- **Verify** — how you'd prove this phase works with nothing after it built. Concrete: the test that must pass, the command whose output you'd check, the thing you'd observe.
- **Checklist** — `[ ]` sub-items the builder ticks.

Keep them lean. Resist adding fields.

Phase heading format:

```markdown
### Phase 2 — Razorpay + webhook · deps: P0 · —
```

## The upkeep block

Every spec ends its phase section with concise maintenance rules so the implementer keeps it accurate without recording routine history. Adapt project conventions to the repository.

```markdown
### Conventions (all phases)
- <project conventions found during exploration: lint, test layout, codegen, style>
- Run <lint> + <test> before marking a phase done.
- **Contract freeze:** <interfaces fixed in the first phase>. Changing them means updating this spec first, then telling dependent phases.

### Keeping this spec current
- Update the status marker on the heading and tick the checklist as you go.
- Update the plan to current truth. Add one brief deviation note only when behavior, scope, contracts, or phase boundaries materially change.
- After a phase lands, add only detail that would surprise the next reader. Skip anything the code already says plainly.
- Put unresolved problems in Open Decisions or a Follow-up note with enough detail to act later. Do not fix them outside the requested phase.
```

## Anti-patterns

- **Restating the requirements back.** If a bullet could have been written without reading the codebase, it's not earning its place.
- **Phases split by layer.** "Phase 1: models. Phase 2: services. Phase 3: API." None of those ship alone. Split by shippable capability.
- **Verify criteria that say "tests pass".** Say which behavior the test proves.
- **A Decisions section that reads as a summary.** It's the record of what was resolved during the interrogation, including the ones the user overruled you on.
- **Questioning past the decision point.** Stop when remaining uncertainty does not materially change implementation.
