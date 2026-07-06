---
name: x-autodm-preflight
description: >-
  Run a gated pre-launch check before any "reply WIFI" AutoDM trigger post fires
  for @BowTiedYanqui. Use this skill whenever the user says "preflight the WIFI
  post," "AutoDM check," "is the funnel ready," "check before the trigger post,"
  "run the preflight," or mentions scheduling/publishing a reply-with-WIFI post
  that fires the AutoDM funnel. This gates a LIVE monetization funnel (the free
  "WiFi Escape Plan" guide), so every check must PASS or the skill returns "DO
  NOT POST" with the specific fix. It checks the DM screenshot workaround, X DM
  settings, the exact trigger keyword, one-per-day cadence, angle rotation, and
  format compliance. The skill never posts, schedules, or edits anything itself.
  Prefer this skill any time a WIFI trigger post is about to go into the queue —
  a broken funnel means lost leads.
---

# x-autodm-preflight

Gate a reply-WIFI AutoDM trigger post before it goes live. This is the front
door of a real funnel: a trigger post that fires when the AutoDM is misconfigured
sends readers a dead link or nothing at all, burning the highest-intent leads the
account gets. So this skill is pass/fail by design — any single FAIL means "DO
NOT POST." It never posts, schedules, or edits; it only clears or blocks. The
executing model does nothing unprompted; run every check and produce the step-7
output.

## Project check

Before anything else, confirm you're running inside the user's X Growth Claude
Project (look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in context). If it
isn't loaded, stop and say: "This skill is designed to run inside your X Growth
Claude Project, where your account context, niche, and voice instructions live.
Open that project and run this skill from there." Don't proceed until confirmed.

## Browsing rules (hard rules — needed for any live check in steps 2 and 4)

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
  keying on the first 60 characters of each post segment. Corrupted numbers mean
  a post block split across a virtualization boundary — re-scroll.
- **On any queue/published date pull, set the end date one day forward.** A same
  start/end date silently drops late-day posts. Navigate with explicit URL date
  parameters, not the UI calendar controls. TweetHunter published tab:
  `app.tweethunter.io/queue?tab=published`; schedule tab:
  `app.tweethunter.io/queue?tab=schedule`.

## Format rules (used in step 6)

A post passes format only if ALL hold: no em dashes (—); no hashtags; max 2
sentences per paragraph; the final line is exactly one closing question; no AI
filler phrases.

## The three funnel angles (used in step 5)

- **(a) Discovery** — the location-independence moment (the realization you could
  live/work from anywhere).
- **(b) Skills** — the IT-to-remote transition (how the technical skill enabled
  the move).
- **(c) Lifestyle payoff** — earnings vs. local cost of living.

## Procedure

Run every check in order. Each ends in **PASS** or **FAIL**. Do not stop at the
first FAIL — run all seven so the user sees the full picture, but a single FAIL
determines the verdict.

1. **Screenshot workaround check.** X malforms guide URLs in DMs — it drops the
   leading "h" in "https", making the link dead. The fix is sending the guide as
   a screenshot/image in the AutoDM, not a bare URL. Confirm — with the user, or
   in TweetHunter's AutoDM configuration — that the current AutoDM reply includes
   the image workaround and does NOT rely on a raw URL. **FAIL if unconfirmed —
   never assume it's set.**

2. **DM settings check.** X must allow message requests from **Everyone** (not
   verified-only), or the AutoDM can't reach non-followers. Verify via
   `x.com/settings/direct_messages` (browsing rules apply) or ask the user to
   confirm. **FAIL if unknown.**

3. **Trigger keyword check.** The post copy must ask readers to reply with the
   exact keyword **WIFI**, matching the AutoDM trigger configuration. **FAIL** if
   the copy uses a different word, a variant, or no explicit keyword.

4. **Cadence check.** Max **one** WIFI trigger post per day. Check today's queue
   (schedule tab) and the published tab for another trigger post today. **FAIL if
   one already exists** today.

5. **Angle rotation check.** Identify the most recent trigger post's angle among
   the three funnel angles above, then confirm today's post uses a **different**
   angle. **FAIL if it repeats** the most recent angle.

6. **Format check.** Run the post against the Format rules above. **FAIL** on any
   violation, quoting the offending text.

7. **Output.** A checklist with **PASS/FAIL per item (1–6)**, then the verdict:
   - **Any FAIL → verdict "DO NOT POST"**, listing each failed item with its
     specific fix.
   - **All PASS → verdict "Cleared to post"**, plus the reminder: "You review
     before anything enters the queue — this skill doesn't post or schedule."
   Then offer a **memory-ready summary block** (fenced). Offer it — do NOT write
   to memory automatically.

## Worked example (one FAIL case)

Input: "Preflight the WIFI post for today. Copy: 'Three years ago I was fixing
servers in a cubicle. Today I answered emails from a rooftop in Palermo. The
skill that got me here is more portable than you think. Reply WIFI and I'll send
you the exact guide I used to make the jump.'"

Checklist:
```
1. Screenshot workaround ...... PASS — confirmed with user: AutoDM sends the
   guide as an image, no raw URL.
2. DM settings (Everyone) ..... PASS — x.com/settings/direct_messages shows
   "Allow message requests from everyone" enabled.
3. Trigger keyword "WIFI" ..... PASS — copy says "Reply WIFI."
4. Cadence (1/day) ............ PASS — no other trigger post in today's schedule
   or published tab.
5. Angle rotation ............. FAIL — today's post is angle (b) skills
   (IT-to-remote transition). The most recent trigger post (yesterday) was also
   angle (b). This repeats the angle.
6. Format ..................... PASS — no em dashes, no hashtags, paragraphs ≤ 2
   sentences, final line is one question.
```

Verdict: **DO NOT POST.**
Fix: Rotate the angle. Yesterday used (b) skills; today should use (a) discovery
(the location-independence moment) or (c) lifestyle payoff (earnings vs. local
cost of living). Rewrite the hook to that angle and re-run the preflight.

Memory-ready block (offered, not written):
```
[Current state] AutoDM preflight run for today's WIFI post → DO NOT POST. Sole
failure: angle rotation (repeated angle (b) skills two days running). All other
checks passed. Pending: rewrite to angle (a) or (c) and re-preflight.
```

## Reminders

- Every check ends PASS or FAIL. One FAIL = "DO NOT POST."
- Never assume the screenshot workaround or DM settings — confirm or FAIL.
- The keyword is exactly WIFI. One trigger post per day. Rotate the angle.
- This skill never posts, schedules, or edits. It clears or blocks.
- Offer the memory block; never write memory yourself.
