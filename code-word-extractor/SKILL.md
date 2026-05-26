---
name: code-word-extractor
description: Extract a working community lexicon from a corpus — value-words, tension-words, charged nouns, code phrases, structural patterns, inside language — into a reusable per-community word list. USE when preparing to write for a specific audience and you do not yet have their lexicon, or when refreshing an existing one. Companion to value-writing. Accepts URLs, PDFs, pasted text, or folders as corpus input. The lexicon file is written outside this skill, to a location the user specifies.
license: MIT
---

# Code-Word Extractor

Companion to **value-writing**. Builds a per-community lexicon a writer
pastes from at revision time.

## When this skill fires

Triggers when the user wants to extract or refresh a code-word list for a
target community. Usually invoked before writing for a new audience, or
when an existing list feels stale.

Does NOT fire for: generic style guides, dictionary lookups, or "make this
sound smarter."

## What it produces

A markdown lexicon file with categorized entries. The location is the
user's call — somewhere in their project workspace, not inside this skill
directory.

Default file structure:

```markdown
# <Community> code-word list

Last updated: <date>
Corpus sources: <list of URLs / files / pasted blocks>

## Community-signaling
- word — (frequency in corpus) — short note on usage if needed

## Tension-signaling
- word — (frequency) — note

## Charged nouns / verbs
- ...

## Code phrases / inside language
- phrase — what it signals to the community

## Structural patterns
- pattern — example from corpus

## Continuity / kill list
- words to AVOID — they signal outsider register

## Hedges / kill list
- words to AVOID — they drain tension

## LLM-suggested (FLAGGED, not derived from corpus)
- word — note on why it likely fits but did not appear in the corpus
```

## Process

### Step 1 — Confirm community
Ask the user, briefly:
1. Who is the community? (a named audience, not "founders")
2. What do they read? (the corpus you are about to provide)
3. What are you planning to write for them?

If the answers are generic, push back once. Then proceed.

### Step 2 — Ingest the corpus
Accept any of:
- URLs (fetch them)
- PDFs (read them)
- Pasted text blocks
- A folder of files

Mix is fine. Aim for 5–15 documents minimum to see patterns. Fewer is
acceptable for niche communities, but flag low confidence in the output.

### Step 3 — Extract candidates
Walk the corpus and collect:

- **Community-signaling words**: prove the community cares about the
  topic ("widely," "established," "the consensus," "well-known," named
  authorities cited frequently).
- **Tension-signaling words**: destabilize the settled state ("however,"
  "but," "puzzlingly," "anomaly," "inconsistent," "in tension with,"
  "what's been less examined," "the open question").
- **Charged nouns and verbs**: cognitive-weight vocabulary specific to
  this community ("moat," "compounding," "wedge," "PMF-or-die,"
  "vibe-coded," etc., depending on community).
- **Code phrases / inside language**: multi-word expressions that signal
  in-group fluency ("design partner," "land-and-expand," "DRI," "P0,"
  "north-star metric," etc.).
- **Structural patterns**: recurring opener templates, parallelism slots,
  rhetorical structures used by writers in this venue.
- **Continuity vocabulary**: words to AVOID because they read as outsider
  / academic / corporate ("extends," "builds on," "consistent with,"
  "as previously established," "in line with").
- **Hedges to avoid**: "perhaps," "consider," "might be worth," "could
  potentially."

For each candidate, track:
- Surface frequency (how many corpus docs it appears in).
- Whether it appears at high-value positions (sentence ends, openers,
  pull quotes, headers).

### Step 4 — Cluster and rank
For each category, rank by corpus frequency × position-weight. Top 10–20
per category is plenty.

### Step 5 — LLM-knowledge augmentation (FLAGGED)
Cautiously add candidates from general knowledge of the community that
did not appear in the corpus but plausibly belong. ALWAYS flag these in
a separate "LLM-suggested" section. Never mix them into corpus-derived
sections. The writer decides whether to promote.

### Step 6 — Emit the lexicon
Write the file to a path the user specifies. If updating an existing
list, diff: show added candidates and changed frequencies. Do not
silently overwrite.

## Update mode

When invoked against an existing lexicon, treat the existing list as
ground truth. Show:
- New corpus-derived candidates with frequency.
- Existing entries whose frequency changed materially.
- New LLM-suggested candidates (FLAGGED).

Do not auto-remove entries from the existing list. The writer prunes.

## Output format

Always:

1. Brief report on corpus stats (document count, total word count, spread
   across categories).
2. The lexicon file written to disk at the user-specified path, or the
   diff if in update mode.
3. 3–5 representative quotes per category from the corpus (sentence-level)
   so the writer can see usage in context.
4. A confidence note: how many docs, how much signal vs noise, where the
   LLM-augmented additions came from.

## Tone

Workmanlike. Concise. Not poetic. The output is a tool, not an essay.
