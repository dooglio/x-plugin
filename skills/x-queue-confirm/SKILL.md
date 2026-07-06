---
name: x-queue-confirm
description: >-
  Verify the final state of the @BowTiedYanqui TweetHunter queue against what was
  drafted this session, then output a locked-queue summary. Use this skill
  whenever the user says "confirm the queue," "lock the queue," "verify my
  scheduled posts," "check the queue matches," "does the queue match what we
  drafted," or pastes one or more TweetHunter preview URLs
  (https://tweethunter.io/public?id=...). Also trigger before a posting week
  starts when the user wants a final check that scheduled posts match the
  approved drafts, are free of duplicates and thematic overlap, and pass the
  format rules. This skill fetches preview URLs, diffs final text against drafts,
  flags TweetHunter's known triple-publish bug, checks format compliance, and
  outputs a locked-queue table plus a memory-ready summary. It never edits or
  schedules anything. Prefer this skill any time the queue needs a final
  pre-publish verification.
---

# x-queue-confirm

Verify that what's actually scheduled in TweetHunter matches what was approved,
and produce a locked-queue summary the user can trust. The queue is the last
gate before posts go live to the audience, so the job is verification and
flagging — never editing, never scheduling. The executing model does nothing
unprompted; follow the numbered steps and produce the step-8 output.

## Project check

Before anything else, confirm you're running inside the user's X Growth Claude
Project (look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in context). If it
isn't loaded, stop and say: "This skill is designed to run inside your X Growth
Claude Project, where your account context, niche, and voice instructions live.
Open that project and run this skill from there." Don't proceed until confirmed.

## Browsing rules (hard rules — only needed if you read the LIVE queue in step 7)

Steps 1–6 use `web_fetch` on preview URLs and need no browser. Only step 7
(reading the live TweetHunter queue) touches the browser. When it does, obey
these rules — they exist because X and TweetHunter break naive automation:

- **Connect once:** `list_connected_browsers` → `select_browser` →
  `tabs_context_mcp` with `createIfEmpty: true`. Capture the `tabId` and pass it
  explicitly to every subsequent browser call.
- **Never scroll with the computer tool.** Use `javascript_tool`: find the real
  scroll container (a `div` where `scrollHeight > clientHeight * 1.2` and
  `clientHeight > 300` — never `document.body` on X), and drive it with
  `container.scrollTop += container.clientHeight * 0.8`, waiting ~450ms between
  steps, max 4–6 steps per `javascript_tool` call.
- **Read content via `javascript_tool`/`innerText`, never visually.**
- **X/TweetHunter lists are virtualized:** only viewport-visible posts exist in
  the DOM. Collect `innerText` at each scroll step into an accumulator and dedupe
  by keying on the first 60 characters of each post segment. Corrupted or
  impossible numbers mean a post block split across a virtualization boundary —
  re-scroll to reassemble it.
- **Reading a queued post:** use the eye/preview icon to read a post. **NEVER
  click the pencil/edit icon to read** — it removes the post from its queue slot.
  Published posts' source of truth is `app.tweethunter.io/queue?tab=published`;
  the live schedule is `app.tweethunter.io/queue?tab=schedule`.

## Format rules (used in step 6)

A post passes format only if ALL hold:

- No em dashes (—).
- No hashtags.
- Max 2 sentences per paragraph.
- The final line is exactly one closing question (the reply trigger).
- No AI filler phrases ("in today's fast-paced world," "let's dive in," "unlock,"
  "game-changer," "it's not just X, it's Y," etc.).

## Procedure

1. **Collect inputs.** Gather (a) the drafted post texts from this session, and
   (b) the user's TweetHunter preview URLs (format
   `https://tweethunter.io/public?id=[UUID]`). If either is missing, ask the user
   to paste the drafts and/or the preview URLs before continuing. These are the
   only inputs you may ask for.

2. **Fetch each preview URL with `web_fetch`** (no browser needed) and extract,
   per post: the final post text, the scheduled date/time, and any media
   reference (image/video attached). If a preview URL fails to fetch, note it and
   move on — don't substitute guesses.

3. **Diff final text vs. draft, verbatim.** For each post, compare the fetched
   final text against its draft. Report any differences by quoting old and new
   text exactly. The user's TweetHunter edits are intentional voice corrections —
   present them as noted changes, never as errors or mistakes.

4. **Duplicate check.** Scan for the same or near-identical text scheduled more
   than once. TweetHunter has a confirmed bug (Jun 2026) that can publish one
   post three times as separate originals. If you find any duplicate or triple,
   flag it loudly at the top of the output as a likely bug instance, listing the
   scheduled slots involved.

5. **Thematic-overlap check.** Flag posts with overlapping themes scheduled on
   the same or adjacent days (e.g., two food posts back-to-back, two "cost of
   living" posts a day apart). Name both posts and the shared theme.

6. **Format check.** Run every post against the Format rules above. Report each
   violation with the offending text quoted.

7. **Live-queue read (only if needed).** If the preview URLs don't cover the full
   queue, or the user wants the live schedule verified, read it in the browser
   under the Browsing rules above. Use the eye/preview icon; never the pencil.
   Cross-check the live schedule against steps 2–6.

8. **Output.** Produce, in this order:

   - Any loud flags first (duplicate/triple-publish bug, format failures).
   - A **locked-queue table**, one row per post:

     ```
     | Date | Time | First line | Trigger Q? (Y/N) | Flags |
     ```

   - A **memory-ready summary block** (fenced), mapping to the user's memory
     sections (Current state / On the horizon). Offer it — do NOT write to memory
     automatically. Example: "Queue locked for week of Jul 7: 5 posts confirmed,
     1 draft/final diff noted (intentional), 0 duplicates, 0 format failures."

## Worked example

Input: user pastes three drafts and three preview URLs; asks "lock the queue."

After fetching the three previews:

Loud flags:
```
⚠ DUPLICATE / possible TweetHunter triple-publish bug: the "asado Sunday" post
  appears in TWO schedule slots — Tue Jul 8 9:00am AND Wed Jul 9 9:00am. Verify
  and delete one before publishing.
```

Diff report:
```
Post 2 (colectivo story): final text differs from draft.
  DRAFT: "...and the driver just waved me on. Wild."
  FINAL: "...and the driver just waved me through. Small win."
  → Noted as an intentional voice edit, not an error.
```

Locked-queue table:
```
| Date       | Time   | First line                         | Trigger Q? | Flags            |
| Tue Jul 8  | 9:00am | "Sunday asado with the neighbors"  | Y          | DUPLICATE (Wed)  |
| Tue Jul 8  | 2:00pm | "Took the colectivo across town"   | Y          | —                |
| Wed Jul 9  | 9:00am | "Sunday asado with the neighbors"  | Y          | DUPLICATE (Tue)  |
```

Memory-ready block (offered, not written):
```
[Current state] Queue for week of Jul 7 reviewed. Flagged: asado post double-
scheduled (Tue + Wed) — likely TweetHunter triple-publish bug; needs one slot
deleted. Colectivo post has an intentional voice edit vs. draft. No format
failures. Not yet "locked" — pending the duplicate fix.
```

## Reminders

- Verify and flag only. Never edit or schedule.
- Preview URLs = web_fetch. Live queue = browser under the hard rules.
- Eye icon to read; the pencil icon deletes the post from its slot.
- Draft/final diffs are intentional voice edits — note, never scold.
- Offer the memory block; never write memory yourself.
