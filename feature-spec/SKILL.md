---
name: feature-spec
description: Run the full spec process for a feature — explore the codebase, interrogate the user with batched multiple-choice questions until every material ambiguity is resolved, then write a dense spec.md with locked decisions, rejected alternatives, and independently-shippable implementation phases. Use this whenever the user wants a spec, design doc, technical plan, RFC, or implementation plan written to a file, and also when they describe a feature they want built and the work is large enough to need phasing, even if they never say the word "spec". Triggers include "write a spec for X", "spec this out", "plan the implementation", "break this into phases", "design doc for", "how should we build X", or pasting a pile of requirements and asking what to do with them. Prefer this over ad-hoc planning whenever the deliverable is a markdown plan that a later agent will build from. Skip it for single-file bug fixes and changes small enough to just make.
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

### 1. Explore (parallel subagents)

Before asking anything, find out what's actually there. Fan out read-only agents — one per area, in a single message so they run concurrently. Typical splits: the subsystem being changed, the code that would be reused or deleted, adjacent systems that will feel the change, existing conventions (test layout, migration style, lint config), prior art elsewhere in the repo.

First check whether the feature crosses repos. Sibling directories, submodules, an infra repo next to an app repo — if the feature touches more than one, explore each. Ask if unsure.

Ask each agent for: what exists, what's reusable, what's in the way, what surprised you. That last one is the point.

### 2. Report findings

Show a short summary before the first question — 10-20 bullets, grouped. What exists, what's reusable, what blocks, what surprised you. This calibrates the user before they answer anything, and it lets them correct a wrong read of the codebase before it poisons every downstream question.

### 3. Interrogate

Use the multiple-choice question tool. Four questions per round, several rounds. Each option gets a real tradeoff in its description — the user is picking between consequences, not labels. Where you have a view, put your recommended option first and mark it `(Recommended)`.

Ask about what actually changes the design:

- Ambiguities in the requirements with two or more defensible readings
- Scope boundaries — what's deliberately *not* in this
- Anything that constrains the data model or the contract between components
- Failure behavior: what happens when the external thing is down, slow, or lies
- Migration and backfill for anything already in production
- Anything where you'd otherwise be guessing at the user's business context

Don't ask what the codebase already answers, and don't ask about things whose answer wouldn't change a single line of the spec.

Keep going until nothing material is unresolved. Before the last round, say what you think is still open so the user can add to it. Genuinely open questions that don't block the build get parked in Open Decisions — that's a legitimate destination, not a failure.

### 4. Pick the path, then write

Ask where the file goes unless the user already said. Look for an existing convention first (`design/`, `docs/`, `specs/`) and propose a path matching it.

Then think hard about phases before writing a line of the phase section. This is the part that most rewards slow thinking — see Phase design below.

Write the file. Don't summarize it back in chat; the file is the deliverable. Say where it is and flag anything you're unsure about.

### 5. Iterate

The user reads and reacts. Revise the file. Repeat until they're satisfied.

## Structure

Mandatory: **Goal**, **Scope (In/Out)**, **Decisions**, **Considered & rejected**, **Implementation phases**, **Open Decisions**, **Risks**.

Everything else is by need. Design sections between Decisions and phases are where most of the spec lives, and their shape follows the feature — data model, endpoints, state machine, the loop, the flow. Name them for what they are. Don't invent sections to fill a template, and don't drop a section the feature obviously needs because it isn't on the mandatory list.

`Considered & rejected` can be its own section or inline where the decision is made — inline is often better, since the rejected option is most interesting next to the one that won. Either way it must exist somewhere.

Tables, ASCII diagrams, short code snippets, and file lists are all fair game — none are required. Use one when it genuinely beats bullets: a state machine as a diagram, endpoints as a table, the exact buggy line as a snippet. Reach for prose bullets by default.

## Writing rules

Short bullets. No filler. No hedging. No "we should consider" — decide, or park it in Open Decisions.

Write it the way you'd brief a peer who is competent and busy: assume they know the stack, don't explain what a webhook is, don't pad transitions. A line either changes what someone builds or it comes out.

Name real things — `services/auth.py:88`, `SignupSession.status`, `map.json`. Vague references ("the auth service") make a spec unbuildable.

## Phase design

A phase is a stopping point, not a work chunk. The test: if the project were cancelled the moment this phase landed, would the system be in a coherent state? If no, the boundary is wrong.

Phase one is usually whatever unblocks everything else — a contract, a schema, a bug that must be fixed before anything downstream is safe. Sequencing after that follows dependencies, not effort.

Prefer parallelism where the deps allow it, and say so explicitly — a dependency graph plus which phases run alongside each other. Note where the graph lies: two phases can be independent on paper and sequential in practice (a codegen step, a shared migration, a deploy that must land first). Call that out; it's exactly the kind of thing that burns a parallel build.

Each phase carries:

- **Status marker** on the heading. `— ` (not started) → `IN PROGRESS` → `CODE DONE, VERIFICATION PENDING` → `✅ DONE`. Add a qualifier when the truth needs one.
- **Deps + parallelism** — `deps: P1, P2 · ∥ P4`.
- **Deliverables** — bullets naming files/functions and the approach. What and how.
- **Verify** — how you'd prove this phase works with nothing after it built. Concrete: the test that must pass, the command whose output you'd check, the thing you'd observe.
- **Checklist** — `[ ]` sub-items the builder ticks.

Keep them lean. Resist adding fields.

Phase heading format:

```markdown
### Phase 2 — Razorpay + webhook · deps: P0 · ∥ P1, P4 · —
```

## The upkeep block

Every spec ends its phase section with the rules for maintaining itself, so the agent that builds from it knows what to write back. Adapt the project conventions to the repo you explored; keep the upkeep rules as they are.

```markdown
### Conventions (all phases)
- <project conventions found during exploration: lint, test layout, codegen, style>
- Run <lint> + <test> before marking a phase done.
- **Contract freeze:** <the interfaces fixed in the first phase>. Changing them means updating this spec first, then telling dependent phases.

### Keeping this spec current
- Update the status marker on the heading and tick the checklist as you go.
- When the build deviates from the plan, **strike the original line and say why it changed** — `~~original~~ **Cut in P4.** <reason>`. Never silently rewrite; the reason a plan changed is worth more than the plan.
- After a phase lands, add only detail that would surprise the next reader — a constant whose value is load-bearing, a behavior that isn't what the name suggests, an ordering that matters. Skip anything the code already says plainly.
- Problems found but not fixed go to Open Decisions or a Follow-up note, with enough detail to act on later. Don't fix them inline and don't leave them unrecorded.
```

## Template

```markdown
# <Feature> — Spec

## Goal
- <one or two lines: what changes, and the shape of the flow>

## Scope
- **In:** <...>
- **Out:** <...>

## Decisions
- **<Question>:** <decision>. <why, only where non-obvious>
- ...

## <Design sections — shaped by the feature>
<data model / endpoints / state machine / the loop / flow — whatever this feature actually needs>

## Considered & rejected
- **<Alternative>** — <why it lost>
<or fold these inline next to the decisions they lost to>

## Implementation phases

<dependency graph, if there's parallelism>

### Conventions (all phases)
<...>

### Keeping this spec current
<...>

### Phase 0 — <name> · deps: none · blocks all · —
- <deliverable>
- **Verify:** <how you'd prove it standalone>
- Checklist:
  - [ ] <item>

### Phase 1 — <name> · deps: P0 · ∥ P2 · —
...

## Open Decisions
- <deliberately deferred, with what would resolve it>

## Risks
- **<Risk>** — <consequence>. Mitigation: <...>

## Success criteria
- <end-to-end statements that must be true when all phases land>
```

## Anti-patterns

- **Restating the requirements back.** If a bullet could have been written without reading the codebase, it's not earning its place.
- **Phases split by layer.** "Phase 1: models. Phase 2: services. Phase 3: API." None of those ship alone. Split by shippable capability.
- **Verify criteria that say "tests pass".** Say which behavior the test proves.
- **A Decisions section that reads as a summary.** It's the record of what was resolved during the interrogation, including the ones the user overruled you on.
- **Writing before the questions are exhausted.** A spec built on assumptions is worse than no spec — it gets built.
