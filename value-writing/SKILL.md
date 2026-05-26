---
name: value-writing
description: Critique and rewrite non-academic, non-fiction prose using a reader-first, problem-construction framework adapted from Larry McEnerney's University of Chicago writing program. USE when writing, drafting, editing, reviewing, polishing, or rewriting cold outbound emails, sales copy, landing pages, marketing copy, blog posts, essays, newsletters, internal memos, proposals, RFCs, decks, or pitch docs. The skill BLOCKS on an interrogation of reader + problem + stakes + action before producing any draft or critique. Companion files (tactics.md) loaded on demand. A sibling skill (code-word-extractor) builds per-community lexicons. SKIP for code, commit messages, PR descriptions, fiction, poetry, and academic papers — warn but proceed if asked to apply it there.
license: MIT
---

# Value Writing

Prose has no inherent value. Value is conferred by readers. A piece of
writing succeeds when specific readers feel that something they care about
was unstable, missing, or wrong — and that the text resolves it. Everything
else is decoration.

This skill is a translation of Larry McEnerney's "Problem of the Problem"
into non-academic, action-oriented writing: outbound emails, marketing
copy, blog posts, memos, proposals, decks.

## Companion files

Load only when the work calls for them; do not auto-read.

- **tactics.md** — sentence mechanics, word-level moves, named structural
  patterns (Manifest/Critical, negation, working-backward, layered
  tensions, parallelism-with-tension), coherence techniques, the 10% rule.
  Reach for it during rewrite when surface-level fixes need precision.


A sibling skill, **code-word-extractor**, builds per-community lexicons
from a corpus (URLs, PDFs, pasted text, folders). Suggest it informally in
Stage 1 if the writer does not already have a code-word list.

## When this skill fires

Triggers on any prose-writing task: write, draft, edit, review, polish,
rewrite, critique. Includes outbound, marketing, blog, memos, proposals,
newsletters.

Does NOT fire for: code, commit messages, PR descriptions, fiction, poetry,
academic papers, song lyrics. If asked to apply it to those anyway, note
the mismatch in one sentence and proceed.

## Stage 1 — Interrogate before you write a single line

Never produce a draft, critique, or rewrite until the writer has answered
these. If they tried to skip, push back. If their answer is generic
("founders", "engineers", "people who care about X"), push back harder.

Ask them, in order, and do NOT proceed until each is concrete:

1. **Who, specifically, is the reader?** Not a segment. A person with a
   title, a recent experience, a current preoccupation. "Series A CTOs
   who just had an outage last quarter" beats "engineering leaders".

2. **What is unstable, missing, or wrong in their world right now?** State
   it as something *they* would recognize — not something *you* find
   interesting. "Your on-call team can't tell which deploy broke prod"
   beats "observability is fragmented".

3. **Have they noticed it?** If yes — what surfaced it for them? If no —
   what is your evidence that it's actually their problem, and not just
   one you find interesting? Writers wildly overestimate how much readers
   share their preoccupations.

4. **How big is it for them?** What does leaving it unresolved cost them
   this week, this quarter, this year? Time, money, headcount, sleep,
   reputation, optionality? Make it specific. "Costs them an engineer-week
   per incident" beats "slows them down".

5. **What action do you want?** A reply? A meeting? A signup? A reshare?
   An internal decision? Be exact. "Twenty-minute call next Tuesday" beats
   "explore working together".

6. **Why would a skeptical version of this reader take that action?** Not
   why you'd take it. Not why your believer-friend would. Why would the
   person who already hates outbound, or already tried three vendors, take
   it. If the answer is "because the product is good" — you do not have
   an answer yet.

7. **Why you, why now?** What is the specific reason this should land
   *with this reader*, *this week*, from *you* — and not from anyone else,
   or six months ago, or six months from now.

If any answer is vague, name it vague and ask again. You are not being
annoying — you are saving them from writing a text that fails.

**Informal check (non-blocking)**: ask once whether the writer has a
code-word list for this community — the lexicon they should be pulling
from at revision time. If not, mention that the **code-word-extractor**
sibling skill can build one from a corpus. Do not block on it.

**Dynamic follow-ups**, asked only when the specific task needs them
(use judgement — don't ask all three on every task):
- *Value code*: does this community respond to **pragmatic** problems
  ("what this lets us do") or **conceptual** problems ("what this lets us
  understand")? Same content, two framings. Read 2–3 published pieces in
  their venue and match.
- *Delta*: where is the reader now, where are you moving them, and what's
  the smallest plausible delta this text can achieve? A cold email rarely
  converts a denier into an advocate — sometimes the win is "make
  opposition harder," not "create a supporter."
- *Inside or outside the reader?*: is the problem you're constructing
  placed in the world (which the reader can disclaim) or in the reader's
  own lap (which they can't)? See tactics.md.

## Stage 2 — Diagnose the draft

Once you have a draft, scan for these failure modes and call each out by
line or phrase. Be blunt. Cold and stony, not warm and fuzzy.

- **Background dump before value.** Opens with history, market context,
  definitions, or "as you know" — instead of opening with something the
  reader feels is broken.
- **Stability where there should be instability.** Writer tried to *settle*
  the reader (calm intro, definitions, scene-setting) when they should
  have *unsettled* them.
- **Problem in the world, not in the reader.** "Observability is hard" is
  a problem in the world. "Your dashboards are lying to you" is a problem
  in the reader. Texts succeed on the second; they get ignored on the
  first.
- **Gap without stakes.** "Here is a thing you don't know." So what? If
  the gap has no cost, filling it has no value. Gap-filling fails because
  knowledge isn't a finite puzzle with empty squares — it's an open
  conversation. Filling one gap among infinite gaps creates no value.
  Error-claims beat gap-claims because the reader has skin in the error.
- **Thesis instead of problem.** Opens by announcing what the text is
  about or what it argues, instead of constructing a problem the reader
  feels. Readers don't engage with theses; they engage with problems that
  demand solutions.
- **Writer-centric content.** Says what helped the *writer* understand.
  Says what the *writer* finds interesting. Readers don't care about the
  writer's journey. Root cause: school trained writers to explain in order
  to *show they understand*, not to *change the reader's mind*. That habit
  takes years to break.
- **Vague stakes.** "Better", "improve", "more efficient", "streamline",
  "unlock" — these are stake-shaped placeholders, not stakes. Replace with
  a number, a name, an hour count, a dollar figure, a recognizable
  consequence.
- **No clear action, or a soft action.** "Let me know if interested."
  "Happy to chat." These are wishes, not asks. Replace with a concrete,
  low-friction next move.
- **Hedges that drain tension.** "Perhaps", "consider", "might be worth",
  "could potentially". Cut them. They signal that the writer is not sure
  the reader should care — so the reader concludes the same.
- **Generic openers.** "In today's fast-paced world", "As technology
  evolves", "Every company is now a software company", "Throughout
  history…". Delete on sight.
- **Continuity-vocabulary creep.** "Extends," "builds on," "consistent
  with," "in line with," "as previously established." These come from a
  dead knowledge-as-accumulation model. Hunt-and-replace at revision with
  tension words ("but," "however," "puzzlingly," "in tension with").
- **Stasis treated as background.** Something the writer presented as
  settled context is actually the thing they need the reader to doubt.
  Move it from background to instability.
- **The writing process leaked into the reading process.** The order in
  which the writer figured things out is not the order the reader needs
  to receive them.
- **Subject randomness.** Sentence subjects varying randomly across a
  paragraph destroys coherence. Diagnostic: circle every sentence subject;
  if they shift around with no pattern, the paragraph reads as incoherent
  even when each sentence is fine. See tactics.md.
- **Important words appearing once.** The text's most-important words
  showing up exactly once each. Readers track what's important by what the
  writer keeps coming back to. See tactics.md (the 10% rule).
- **Challenge delivered too overtly.** "You're wrong," "you missed X,"
  "your team is doing this incorrectly." The structural move is right (an
  error claim) but the register is junior. Sophisticated challenge
  attributes the error to the field, the situation, or a widely-held
  assumption — not to the reader individually.
- **Trust deficit ignored.** Default reader stance is "this will waste my
  time." Opening earns each second. If the first sentence does not give
  the reader a reason to keep going, the rest doesn't get read.

## Stage 3 — Fix moves

Translate diagnoses into surgical changes. Preserve the writer's voice.
Don't rewrite for its own sake.

- **Open with instability in the reader.** First sentence should make the
  reader feel a tension they recognize.
- **Lead with stakes.** Within the first 2–3 sentences, the reader should
  know what specifically gets worse if nothing changes, or better if
  something does.
- **Prefer error over gap.** Saying "the thing you believe is wrong" beats
  "here is something you don't know." Errors land harder because the
  reader has skin in them.
- **Construct backward: solution → problem.** Most writers start with the
  solution they want to convey and reason forward. Reverse it: take the
  solution, ask "what problem would this solve?" — open with that problem.
  The highest-leverage move to convert a thesis-driven draft into a
  problem-driven one. See tactics.md.
- **Relocate problem from outside to inside the reader.** "The industry
  has a problem" → reader shrugs. "Your dashboards are lying to you" →
  reader can't shrug. Move the problem from the world into the reader's
  lap.
- **Use negation-then-affirmation** when a positive claim risks landing
  flat. "I thought X — was wrong — here's Y" creates a micro-problem the
  affirmation then resolves. Works for celebration content (founder
  profiles, hiring announcements, "lessons learned") that would otherwise
  feel like puffery. See tactics.md.
- **One concrete detail beats a paragraph of context.** Pick the sharpest,
  most recognizable specific. Cut the rest. Trust the reader.
- **Repeat your most-important words.** Identify the 3 words the text most
  depends on. Repeat each across the piece, or use audience-associated
  cousins. Aim for ~5–10% of total word count concentrated on these.
  Counterintuitive: feels repetitive to the writer, reads as coherent to
  the reader. See tactics.md (the 10% rule).
- **Background goes after value, not before.** Once the reader is bought
  in, you can explain. Same applies to explanation itself: explain only
  inside established value + active persuasion. Professional readers don't
  need teaching; they need persuading.
- **Soften challenge by attribution, not by hedging.** When delivering an
  error claim, attribute the error to the field ("the literature has
  settled on…"), the situation ("teams I've talked to mostly…"), or
  yourself ("what I'd assumed turned out to be backwards") — not to the
  reader individually. Use diagnostic vocabulary ("tension," "anomaly,"
  "what's been less examined") instead of failure vocabulary ("wrong,"
  "missed").
- **Action sentence is concrete and low-friction.** A specific time, a
  specific link, a specific one-line reply. Not an open-ended invitation.
- **Cut hedges, throat-clearing, and closing apologies.** Almost every
  draft is better with the first paragraph deleted.

For sentence-level work, the full word-level toolkit, and named structural
patterns (Manifest/Critical, layered lit-review tensions,
parallelism-with-tension, coherence dimensions), see **tactics.md**.

## Output format

Always: **Diagnosis → Rewrite.**

```
## Diagnosis

- [Line or phrase]: [failure mode] — [why it fails for THIS reader]
- ...

## Rewrite

[Surgical version. Preserves voice. Fixes diagnosed failures. No
explanation embedded in the rewrite — the diagnosis above is the
explanation.]
```

Do not soften the diagnosis. Do not pad it. If the draft has five
problems, list five. If it has one, list one.

## Glossary

Use these terms in the diagnosis when they apply — they are precise:

- **Value** — what readers get from the text. Set by the reader, not the
  writer.
- **Reader** — a specific person with a context, not a segment.
- **Instability** — something the reader feels is broken, missing, wrong,
  tense, unresolved.
- **Stakes (costs/benefits)** — what specifically gets worse if the
  instability stays, or better if it goes. Two distinct codes —
  **cost-language** ("if you don't fix this you lose X") and
  **benefit-language** ("if you fix this you gain X"). Audiences favor one
  or the other; read 2–3 pieces in their venue to detect the pattern.
- **Stasis** — something the writer sets up *in order to destabilize it*.
  Looks like background; functions as a trap.
- **Background** — context the reader needs and will accept as settled.
  Goes *after* value, not before.
- **Gap problem** — "you don't know X." Weak unless paired with stakes.
- **Error problem** — "what you believe about X is wrong." Stronger;
  lands harder.
- **Problem-in-the-world** — an instability in the writer's subject (the
  market, the technology, the situation). Not enough by itself.
- **Problem-in-the-reader** — an instability in what the reader understands
  or can do. Required.
- **Manifest problem** — the obvious problem everyone sees.
- **Critical problem** — the deeper problem that PREVENTS solving the
  manifest one. The "Manifest + Critical" two-problem structure ("yes
  there's X, but we have a second problem that blocks solving X") is a
  high-leverage move for memos, RFCs, and analytical pieces.
- **Value code** — the type of problem a community responds to.
  **Pragmatic** = "what this lets us do." **Conceptual** = "what this lets
  us understand." Same content can be framed either way; mis-coded
  framings get ignored.
- **Action** — the concrete thing you want the reader to do after reading.
  Specific or it doesn't exist.
- **Delta** — the smallest plausible movement this text can achieve.
  Often the realistic goal is "make opposition harder," not "create a
  supporter."
- **Challenge-delivery register** — the linguistic layer of error-claims.
  Structurally correct challenges (praise → but → tension → cost) still
  fail if the words read as overt or sycophantic.
- **Ethics note** — the problem-construction technique works by creating
  anxiety in the reader and positioning the writer as the resolver. The
  anxiety would not have existed if the text hadn't created it. McEnerney
  calls this ethically troubling and so should we. Apply the technique
  knowingly. Sharpen real stakes; don't manufacture fake ones.

## Tone

Be blunt. You are not a cheerleader. The writer can take it. Softening
the diagnosis is a disservice — it leaves them with a draft that will
fail in front of a reader who will not soften anything.

Do not say "great start" or "nice draft" or "this has good bones". Say
what is broken. Then show the fix.
