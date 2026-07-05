---
name: x-session-close
description: >-
  Run a structured end-of-session memory update for the @BowTiedYanqui X account
  workflow so learnings from the session don't get lost. Use this skill whenever
  the user says "close out this session," "session close," "update memory before
  we wrap," "wrap up," "let's finish up," "save what we learned," "update the
  memory file," "end of session," or otherwise signals a working session is
  ending and its durable takeaways should be captured. Also trigger after a
  queue-review, post-drafting, experiment-tracking, or diagnosis session when the
  user wants the results folded into memory. This skill extracts queue changes,
  experiment updates, new learnings, open questions, and corrections; maps them
  to the user's memory sections; filters out noise; and — critically — presents a
  draft and WAITS for explicit approval before anything is written. It never
  writes to memory automatically. Prefer this skill any time a session should end
  with the memory file brought up to date.
---

# x-session-close

Close out a working session by turning what just happened into a clean,
approved update to the project memory file. Sessions generate real signal —
which post got confirmed for Tuesday, what a tracked experiment did on day 3,
a principle that finally clicked — and that signal evaporates if it isn't
written down while it's fresh. This skill captures it without polluting memory
with noise or one-off metrics.

The executing model does nothing unprompted, and one rule is absolute: **you do
not write to the memory file until the user explicitly approves the draft**
(step 4). Everything before that is drafting and filtering; the write happens
only after a clear yes.

## The memory file's sections

The user's project memory uses these exact sections. Map every proposed edit to
one of them:

- **Purpose & context** — what the project is and who it's for (rarely changes).
- **Current state** — where things stand right now (queue status, live
  experiments, active focus).
- **On the horizon** — what's coming up, planned, or pending.
- **Key learnings & principles** — durable, reusable principles.
- **Approach & patterns** — repeatable workflows and how the user likes to work.
- **Tools & resources** — tools, links, accounts, assets in use.

## Procedure

Work through every step in order. Do not write to memory until step 4 returns an
explicit approval.

1. **Review the session and extract items under these exact five headings.**
   Read back over the session and sort what happened into:

   - **Queue changes** — posts added, edited, or confirmed. Include the date and
     scheduled time for each (e.g., "Confirmed asado post for Tue Jul 7, 9:00am
     ART"). If a time or date is unknown, note it as unknown rather than guessing.
   - **Experiment updates** — new numbers for any tracked post (e.g., a day-2 or
     day-3 impression pull), stated with which post and which day.
   - **New learnings** — only durable principles that will apply to future
     sessions. Not one-off observations. (A one-off: "this post did 3k views." A
     durable principle: "photo-led posts outperform text-only ones for this
     account, so lead with an image.")
   - **Open questions** — anything flagged for later, especially claims that
     still need primary-source verification before they can be used or published.
   - **Corrections** — anything already in memory that this session showed to be
     wrong, stale, or superseded.

   If a heading has nothing real under it, write "(none this session)" — don't
   pad it.

2. **Draft the update as edits mapped to the memory sections.** Convert each
   extracted item into a concrete edit and label which section it belongs to
   (Current state / On the horizon / Key learnings & principles / Approach &
   patterns / Tools & resources). When an edit replaces existing memory text,
   show it as old vs. new so the user can see exactly what changes:

   ```
   [Current state] REPLACE
     OLD: "3 posts queued for the week, none confirmed."
     NEW: "5 posts queued; asado post confirmed for Tue Jul 7 9:00am ART."
   ```

   For brand-new material, show it as an addition:

   ```
   [Key learnings & principles] ADD
     "Photo-led posts consistently beat text-only for this account — lead with
      an image whenever raw material allows."
   ```

3. **Apply the filter before presenting.** Remove anything that fails the bar,
   because memory is only useful if it stays dense with decisions rather than
   bloating into a log:

   - **Exclude restatements** — if memory already says it, drop it.
   - **Exclude raw metrics with no decision attached** — a number only earns a
     place if it changed a decision or is an explicit experiment data point
     being tracked over time. "Post got 3k views" alone: cut. "Day-3 pull: 3k,
     confirming the fade pattern we're tracking": keep.
   - **Exclude unlabeled speculation** — if something is a guess, either label
     it as a hypothesis/open question or cut it. Never let speculation enter
     memory disguised as fact.

   State briefly what you filtered out and why, so the user can catch anything
   you wrongly dropped.

4. **Present the draft and WAIT for explicit approval. Never write to memory
   automatically — this is a hard rule.** Show the full proposed update (the
   mapped edits from step 2, after the step-3 filter). Then stop and ask the
   user to approve, edit, or reject. Do not call any file-writing or
   memory-writing tool yet. Ambiguous or partial responses ("looks good but drop
   the last one") mean revise and re-present — only a clear go-ahead counts as
   approval. The reason this is absolute: memory is the account's long-term
   brain, and silently writing an unreviewed or wrong edit is worse than writing
   nothing.

5. **After approval, output the final text in a copy-paste-ready block.** Once
   the user approves, produce the finalized memory text in a single fenced block
   the user can paste straight into their memory file, organized by section
   heading. If a memory-writing tool is available and the user asked you to
   apply it, you may write it now that approval is explicit — but the
   copy-paste-ready block is always produced regardless.

## Worked example (full session close)

Input: "Let's close out this session."

Session recap (what happened): drafted three posts; user confirmed one (an
asado photo post) for Tuesday; pulled day-3 numbers on last week's "expat
banking" post (down to 3k impressions from 11k on day 1); noticed photo posts
keep beating text-only; user wasn't sure whether the current repo still uses the
old 65 Tweepcred threshold and wants it checked before the credibility chapter;
memory currently claims "reply triggers are optional polish," which the session
contradicted.

**Step 1 — extraction:**

- Queue changes: Confirmed asado photo post for Tue Jul 8, 9:00am ART. Two other
  drafts still unconfirmed.
- Experiment updates: "Expat banking" post — day-3 impression pull = 3k (down
  from 11k day-1), consistent with the fast-fade pattern being tracked.
- New learnings: Photo-led posts consistently outperform text-only for this
  account.
- Open questions: Does the current `xai-org/x-algorithm` repo still use the 65
  Tweepcred threshold, or is that a stale 2023 figure? Needs primary-source
  verification before the credibility chapter (hand to x-claim-verifier).
- Corrections: Memory says "reply triggers are optional polish." Session showed
  they're the highest-leverage lever — this is now wrong.

**Step 2 — mapped edits:**

```
[Current state] REPLACE
  OLD: "Reply triggers are optional polish."
  NEW: "Reply triggers are the highest-leverage lever — audit every queued post
        for a missing trigger before it's confirmed."
[Current state] ADD
  "Asado photo post confirmed for Tue Jul 8, 9:00am ART; 2 drafts pending."
[On the horizon] ADD
  "Verify whether the 65 Tweepcred threshold is current in xai-org/x-algorithm
   before writing the credibility chapter."
[Key learnings & principles] ADD
  "Photo-led posts consistently beat text-only for this account — lead with an
   image whenever the raw material allows."
[Approach & patterns] ADD
  "Track flagship posts across day-1/day-2/day-3 impression pulls; the 'expat
   banking' post faded 11k → 3k by day 3, fitting the known fast-fade pattern."
```

**Step 3 — filter applied:**

- Cut: the two unconfirmed drafts' full text (no decision attached yet — they're
  still pending; only the confirmed one and the pending *count* earn a place).
- Kept the 3k day-3 number because it's an explicit tracked experiment data
  point, not a loose metric.
- The Tweepcred item is labeled as an open question, not asserted as fact.

**Step 4 — present and wait:**

"Here's the proposed memory update (above). Want me to finalize it as-is, tweak
anything, or drop items? I won't write anything until you say go."

**Step 5 — after the user says "approved":** output the finalized text in one
copy-paste-ready block, organized by section heading.

## Reminders

- Five extraction headings, six memory sections — map every item.
- Filter hard: no restatements, no decision-less metrics, no unlabeled guesses.
- Show old-vs-new for every replacement.
- Never write to memory before an explicit yes. This rule has no exceptions.
- Hand any "needs verification" open question to x-claim-verifier before it's
  used in the paid product.
