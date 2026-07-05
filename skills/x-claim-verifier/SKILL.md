---
name: x-claim-verifier
description: >-
  Verify a factual claim against primary sources before it goes into "The Claude
  X Growth Guide," a Gumroad eBook, or any paid product. Use this skill whenever
  the user says "verify this claim," "fact-check this," "is this confirmed,"
  "check this before it goes in the book," "can I state this as fact," "does the
  source actually say this," "is this from the current algorithm," or asks
  whether an algorithm signal, weight, multiplier, threshold, or X/xAI platform
  fact is real before publishing it. Also trigger when the user pastes a
  sentence destined for the book and asks whether it holds up, or worries a
  number might be from an old Twitter repo rather than the current
  xai-org/x-algorithm release. This skill fetches
  official X/xAI docs and the actual GitHub source, assigns a confidence tier
  (CONFIRMED / REASONABLE INFERENCE / NOT SUPPORTED), and returns book-ready
  wording calibrated to that tier. Prefer this skill over answering from memory
  any time a claim will be sold to readers.
---

# x-claim-verifier

Verify a single factual claim before it enters a paid product (the eBook, a
Gumroad listing, sales copy). Readers are paying $200 and expect algorithm
literacy, not folklore. A confidently wrong number is a refund and a
credibility hit. Your job is to force every claim through primary sources and
return wording the user can paste in at the correct confidence level.

The executing model does nothing unprompted. Follow the numbered steps in order.
Do not skip a step because the answer "seems obvious" — the entire value of this
skill is that it refuses to trust memory or repetition.

## What counts as a primary source

**Acceptable (can CONFIRM a claim):**

- Official X/xAI documentation — `help.x.com` and other first-party X/xAI docs.
- The `xai-org/x-algorithm` GitHub repository — and specifically the **actual
  source files** (code, config, scorer classes, weight constants). Fetch the
  real file, not the README, not a release blog post, not a summary.
- Official platform announcements published by X or xAI directly.
- First-party product documentation.

**NOT acceptable (can never, on their own, CONFIRM a claim):**

- Aggregator blogs, Medium posts, SEO articles, "algorithm explained" listicles.
- Other AI outputs (including your own prior answers or another chatbot's).
- Screenshots of tweets, or tweets themselves, describing how the algorithm works.
- The comp-title guides (Suzuki's book, LifeMathMoney) — these are for
  understanding *which concepts matter*, never for confirming a fact.

If the only support for a claim is an unacceptable source, the tier is NOT
SUPPORTED — no matter how many places repeat it (see step 7).

## Procedure

Work through every step. Produce the step-6 output block at the end.

1. **Restate the claim in one sentence.** Strip it to a single testable
   assertion. If the user gave you a paragraph, isolate the one factual claim
   being checked. If there are multiple distinct claims, tell the user you will
   verify them one at a time and start with the first.

2. **Identify what would count as a primary source for this specific claim.**
   Name the exact source you'd need to see. Examples: "the scorer weight
   constant in the ranking config file of xai-org/x-algorithm," or "a statement
   on help.x.com about how blocks affect reach," or "an official xAI
   announcement about Premium ranking multipliers." Be specific about the file
   or page — vague sourcing is how stale numbers slip through.

3. **Fetch the primary source(s) with web tools and quote the exact passage or
   code.** Use the web fetch / search tools to pull the real source identified
   in step 2. For GitHub, fetch the actual source file (e.g. the raw file
   contents), not a description of it. Copy the exact relevant sentence or the
   exact line(s) of code — verbatim, in quotes, with the URL. If you cannot
   reach the source, say so plainly; do not substitute an aggregator to fill the
   gap. A claim you could not source is not confirmed.

4. **Assign exactly one of three confidence tiers.** Choose one and only one:

   - **CONFIRMED** — a primary source directly states it. Cite the source and
     include the exact quote or code.
   - **REASONABLE INFERENCE** — the claim is consistent with primary sources but
     no primary source states it directly. Say explicitly what is missing (what
     the source *would* need to say for this to be CONFIRMED, and why it falls
     short).
   - **NOT SUPPORTED** — there is no primary source, or a primary source
     contradicts the claim. If contradicted, quote the contradicting passage.

5. **Version-check any specific numbers.** If the claim names a weight,
   multiplier, ratio, threshold, score, or cutoff, confirm the number came from
   the **current** `xai-org/x-algorithm` codebase or current X/xAI docs — not an
   older source. The 2023 Twitter "the-algorithm" open-source release is a
   different, older codebase; figures from it must not be presented as current
   x-algorithm facts. If you find a version mismatch (or cannot establish which
   version a number came from), flag it explicitly and drop the tier to at most
   REASONABLE INFERENCE until the current-version number is confirmed. State the
   file/commit or doc date you verified against.

6. **Produce the output in this exact format:**

   ```
   CLAIM: <one-sentence restatement>
   TIER: <CONFIRMED | REASONABLE INFERENCE | NOT SUPPORTED>
   EVIDENCE:
     - <exact quote or code> — <source name> — <URL>
     - <version note: which codebase/commit or doc date this was verified against>
   SUGGESTED BOOK WORDING:
     "<text the user can paste in, calibrated to the tier>"
   ```

   Calibrate the suggested wording to the tier, because the hedge has to live in
   the sentence itself, not in a caveat the reader never sees:

   - **CONFIRMED** → state it flatly. ("X ranks replies far above likes: the
     ranking config weights a reply at 27x a like.")
   - **REASONABLE INFERENCE** → hedge inside the text. ("The open-source ranking
     code weights replies heavily over likes, which strongly suggests — though
     the exact real-time weighting isn't published — that driving replies
     matters more than chasing likes.")
   - **NOT SUPPORTED** → do not offer publishable wording. Instead say what the
     user would need to find to make the claim usable, or recommend cutting it.

7. **Never upgrade a tier because a claim is widely repeated.** Repetition is
   not confirmation. If ten blogs and three big accounts all say "the algorithm
   gives video a 2x boost" and no primary source states it, the tier is NOT
   SUPPORTED. Popularity of a claim is evidence about the discourse, not about
   the codebase. Say so directly if the user pushes back with "but everyone
   says this."

## Worked examples

**Example 1 — CONFIRMED**

Input: "In the book I want to say a reply is worth way more than a like to the
algorithm. Verify this before it goes in."

- CLAIM: The X ranking algorithm weights a reply substantially higher than a
  like.
- Source identified (step 2): the engagement weight constants in the ranking
  config of `xai-org/x-algorithm`.
- Fetched the actual config file and quoted the exact reply and like weight
  constants.
- TIER: CONFIRMED.
- EVIDENCE: the quoted weight lines (reply weight >> like weight) with the file
  URL; verified against the current `xai-org/x-algorithm` `main` branch, file
  dated within the current release.
- SUGGESTED BOOK WORDING (flat): "To the ranking model, a reply is worth far
  more than a like — the open-source weights make this explicit. Optimize your
  posts to earn replies, not likes."

**Example 2 — REASONABLE INFERENCE**

Input: "Confirm that dwell time is one of the most important signals so I can
lead the signals chapter with it."

- CLAIM: Dwell time is among the highest-importance ranking signals.
- Source identified (step 2): a feature/signal list or model weighting in
  `xai-org/x-algorithm`, or an official X statement ranking signal importance.
- Fetched the repo: dwell/lingering-style features appear as inputs to the
  ranking model, but the code does not publish a definitive ranked list of
  "most important" signals; real-time weighting is model-driven and not stated
  as prose. No help.x.com page ranks signals by importance.
- TIER: REASONABLE INFERENCE — dwell time is clearly *used* as a signal
  (present in the feature set), but "one of the most important" is not directly
  stated. What's missing: a primary source that ranks signal importance.
- SUGGESTED BOOK WORDING (hedged in-text): "Dwell time is a real, first-party
  ranking signal — you can see it in the feature set the model consumes. The
  code doesn't publish an official ranking of which signals matter most, so
  treat 'dwell time is king' as a well-grounded working assumption, not a
  published fact."

**Example 3 — NOT SUPPORTED (with version trap)**

Input: "I read that Tweepcred below 65 throttles your reach and that verified
accounts get a 4x boost. Both going in the credibility chapter — check them."

- This is two claims; verify one at a time.
- CLAIM A: A Tweepcred score below 65 throttles an account's reach.
- CLAIM B: Verified / Premium accounts receive a 4x ranking boost.
- Version check (step 5): "Tweepcred" and the 65 threshold trace to the **2023
  Twitter `the-algorithm`** release, an older codebase. Presenting a 2023 figure
  as a current `xai-org/x-algorithm` fact is exactly the version mismatch this
  step exists to catch. Fetched the current repo: confirm whether the same
  mechanism/threshold exists in the current release before using it.
- CLAIM B: searched current repo and help.x.com for a "4x" Premium multiplier —
  found no primary source stating a 4x figure. Widely repeated in blogs;
  repetition is not confirmation (step 7).
- TIER (A): NOT SUPPORTED as a *current* fact until the current-version
  threshold is located — flagged as a possible 2023-vs-2026 version mismatch.
- TIER (B): NOT SUPPORTED — no primary source for the 4x number.
- SUGGESTED BOOK WORDING: none for either as written. For A: either find the
  current-release equivalent and re-verify, or write about reputation-style
  scoring conceptually without asserting the 65 number as current. For B: state
  only what a primary source supports (e.g., that Premium affects ranking, *if*
  a first-party source says so) and drop the unsourced 4x.

## Reminders

- One claim at a time. Split compound claims.
- No primary source reached = not confirmed. Never backfill with a blog.
- The hedge lives in the sentence, not in a footnote the reader skips.
- Repetition ≠ confirmation. Old codebase ≠ current codebase.
