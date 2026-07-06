---
name: x-post-experiment-tracker
description: >-
  Track a flagged @BowTiedYanqui post's metrics across multiple days to test a
  growth hypothesis (e.g., "let a banger run" — posting less lets a strong post
  accumulate impressions longer). Use this skill whenever the user says "track
  this post," "day-2 pull," "day-3 numbers," "check the experiment," "how's the
  test post doing," "pull the numbers on that post," or references a running
  multi-day test on a specific post. It reads X account analytics via Claude in
  Chrome, extracts impressions/likes/replies/reposts/bookmarks/profile-visits/
  follows/engagements, computes reply rate, builds a day-over-day delta table
  against prior pulls, classifies the post's reach/resonance state, and renders a
  verdict on whether the hypothesis is supported yet. Also trigger when the user
  wants to schedule the next pull. Prefer this skill any time a specific post is
  being watched over time rather than the account as a whole.
---

# x-post-experiment-tracker

Track one post's metrics over several days to test a hypothesis about how it
behaves. The value is longitudinal: a single snapshot says little, but the shape
of day-over-day gains tells you whether a post is still climbing or has
plateaued — which is what decides whether the experiment is called. The
executing model does nothing unprompted; follow the numbered steps and produce
the step-7 output.

## Project check

Before anything else, confirm you're running inside the user's X Growth Claude
Project (look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in context). If it
isn't loaded, stop and say: "This skill is designed to run inside your X Growth
Claude Project, where your account context, niche, and voice instructions live.
Open that project and run this skill from there." Don't proceed until confirmed.

## Browsing rules (hard rules — this skill always reads live analytics)

- **Connect once:** `list_connected_browsers` → `select_browser` →
  `tabs_context_mcp` with `createIfEmpty: true`. Capture the `tabId` and pass it
  explicitly to every subsequent browser call.
- **Never scroll with the computer tool.** Use `javascript_tool`: find the real
  scroll container (a `div` where `scrollHeight > clientHeight * 1.2` and
  `clientHeight > 300` — never `document.body` on X), and drive it with
  `container.scrollTop += container.clientHeight * 0.8`, waiting ~450ms between
  steps, max 4–6 steps per `javascript_tool` call.
- **Read content via `javascript_tool`/`innerText`, never visually.**
- **X lists are virtualized:** only viewport-visible posts exist in the DOM.
  Collect `innerText` at each scroll step into an accumulator and dedupe by
  keying on the first 60 characters of each post segment. Corrupted or impossible
  numbers mean a post block split across a virtualization boundary — re-scroll to
  reassemble it before trusting a figure.
- **On any analytics date pull, set the end date one day forward.** A same
  start/end date silently drops late-day posts. Navigate with explicit URL date
  parameters, not the UI calendar controls.

## Benchmarks and thresholds (used in steps 2 and 4)

- Account average benchmark: **24,000 impressions**, **0.246% reply rate**.
- Reply rate formula: `replies ÷ impressions × 100` (expressed as a percentage).
- Target reply rate on niche posts: **0.3%+**.
- "Still gaining meaningful impressions" = **>10% day-over-day** growth.

## Classification (step 4) — choose the one that fits

- **Reach + resonance:** reply rate ≥ 0.2% AND impressions above account average
  (>24K).
- **Reach without resonance:** impressions > 3× account average (>72K) BUT reply
  rate < 0.2%.
- **Below benchmark:** neither of the above (state which dimension is short).

## Procedure

1. **Collect inputs.** Gather (a) the post ID or post URL, (b) the hypothesis
   being tested (in one sentence), and (c) any prior pulls (dates + numbers). If
   the user hasn't given prior pulls, ask, or read them from pasted context. If
   there's no post ID/URL, find the post at `x.com/i/account_analytics/content`
   and get its ID (read via `javascript_tool`/`innerText` under the browsing
   rules). These are the only inputs you may ask for.

2. **Pull current metrics.** Navigate to
   `x.com/i/account_analytics/content/[POST_ID]` and extract: impressions, likes,
   replies, reposts, bookmarks, profile visits, new follows, engagements. Compute
   reply rate = replies ÷ impressions × 100. Sanity-check every number against
   the virtualization rule before recording it.

3. **Build a day-over-day delta table** against prior pulls — show both absolute
   values per pull and the per-day gain (Δ) for each metric.

4. **Classify the post's current state** using the Classification rules above.
   State the reply rate vs. the 0.2% / 0.3% targets and impressions vs. the 24K
   average explicitly.

5. **Evaluate the hypothesis.** Is the post still gaining meaningful impressions
   (>10% day-over-day) or has it plateaued? State plainly whether there's enough
   data to call the result:
   - If still climbing >10%/day: verdict **needs day-N** — the experiment isn't
     done; name the specific next pull date.
   - If plateaued (<10%/day) across the latest interval: the result can be
     called — state **supported** or **not supported** against the hypothesis.

6. **Offer to schedule the next pull** as an automated task at a specific time
   (e.g., "Want me to schedule the day-3 pull for tomorrow 9am ART?"). Only offer
   — don't create it unless the user says yes.

7. **Output**, in this order: the delta table, the classification, the hypothesis
   verdict (supported / not supported / needs day-N), and a **memory-ready
   summary block** (fenced) mapped to the user's memory sections. Offer it — do
   NOT write to memory automatically.

## Worked example

Input: "Day-2 pull on the asado post. Hypothesis: if I don't post over it, a
banger keeps accumulating impressions for days. Day-1 was: 31,200 impressions,
128 likes, 94 replies, 22 reposts, 60 bookmarks, 410 profile visits, 38 follows."

Day-2 pull (from analytics): 44,800 impressions, 171 likes, 121 replies, 29
reposts, 78 bookmarks, 540 profile visits, 51 follows.

Delta table:
```
| Metric          | Day 1  | Day 2  | Δ (day-over-day) |
| Impressions     | 31,200 | 44,800 | +13,600 (+43.6%) |
| Likes           | 128    | 171    | +43              |
| Replies         | 94     | 121    | +27              |
| Reposts         | 22     | 29     | +7               |
| Bookmarks       | 60     | 78     | +18              |
| Profile visits  | 410    | 540    | +130             |
| New follows     | 38     | 51     | +13              |
| Reply rate      | 0.301% | 0.270% | —                |
```

Classification: **Reach + resonance.** Day-2 impressions (44,800) are well above
the 24K average, and reply rate (0.270%) clears the 0.2% resonance bar (day-1's
0.301% cleared the 0.3% niche target too).

Hypothesis verdict: **needs day-3.** Impressions grew +43.6% day-over-day — far
above the 10% "still climbing" line — so the banger is clearly still
accumulating. Not plateaued; can't call it yet. Next pull: day-3, tomorrow 9am
ART.

Memory-ready block (offered, not written):
```
[Current state] Experiment "let a banger run" — asado post. Day-1 → Day-2:
31.2K → 44.8K impressions (+43.6%), reply rate 0.30% → 0.27% (still above 0.2%
resonance bar). Reach + resonance. Still climbing >10%/day → not called; day-3
pull scheduled tomorrow 9am ART.
```

## Reminders

- Sanity-check every number against the virtualization rule before trusting it.
- End date one day forward on every date pull.
- >10%/day = still climbing → needs day-N. <10%/day = call it.
- Offer the next-pull schedule and the memory block; write neither yourself.
