---
name: x-morning-checkin
description: Run a daily morning check-in on yesterday's X post performance using Claude in Chrome. Use this skill when the user wants a quick daily read on how yesterday's posts performed — impressions, reply rates, and anything that needs attention. Triggers on phrases like "morning check-in," "how did yesterday do," "check yesterday's posts," "daily check-in," "what happened yesterday," or any request for a same-day performance snapshot.
---

# X Morning Check-In

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

You are running a daily morning check-in on yesterday's X post performance. This is a quick daily read — not a full audit. The goal is to know what moved, what didn't, and whether anything needs follow-up today.

## Browsing rules (hard rules)

- **Connect once:** `list_connected_browsers` → `select_browser` → `tabs_context_mcp` with `createIfEmpty: true`. Capture the `tabId` and pass it explicitly to every subsequent browser call.
- **Never scroll or click through the UI with the computer tool.** Use `javascript_tool`: find the real scroll container (a `div` where `scrollHeight > clientHeight * 1.2` and `clientHeight > 300` — never `document.body` on X), and drive it with `container.scrollTop += container.clientHeight * 0.8`, waiting ~450ms between steps, max 4–6 steps per `javascript_tool` call.
- **Read content via `javascript_tool` / `innerText`, never visually.**
- **Don't use the date-range calendar picker.** It's fragile and some X UI versions won't accept a single-day range. Pull the 7-day window and filter to yesterday's date yourself in step 3.

## Workflow

1. Connect to Chrome and navigate to the 7-day content analytics view, sorted by date descending:

   ```
   https://x.com/i/account_analytics/content?type=posts&sort=date&dir=desc&days=7
   ```

   The user's handle is in the ACCOUNT CONTEXT section of the Project Instructions — the URL above is account-scoped to whoever is logged in, so you don't need to substitute it. Never hardcode a handle.

   If the page was already open on a different sort, the URL parameter alone won't re-sort it. Click the Date header:

   ```js
   const dateSpan = [...document.querySelectorAll('span')]
     .find(el => el.textContent?.trim() === 'Date');
   dateSpan?.parentElement?.click();
   ```

2. Read the rows via `innerText`. Row shape: line 3 = date, middle lines = post text, last 4 lines = Impressions / Likes / Replies / Reposts. Handle `K`/`M` suffixes when parsing.

3. Filter to yesterday's date only — one calendar day before today. Discard everything else.

4. For each of yesterday's posts, record:
   - Post text (first ~10 words is enough to identify it)
   - Impressions, Likes, Replies, Reposts
   - Reply rate: replies ÷ impressions × 100, rounded to two decimal places

## Output format

**Yesterday's Posts** (date: [yesterday's date])

List each post with: impressions, likes, replies, reposts, reply rate.

**Best performer**
Which post got the highest reply rate? One sentence on why it likely worked — trigger, specificity, topic, or structure.

**Worst performer**
Which post had the lowest reply rate (or zero replies)? One sentence on what was missing.

**One thing to act on today**
If a post is gaining traction (replies still coming in, high repost count), note whether it's worth a self-repost or quote today. If yesterday was flat across the board, say so plainly — some days are just slow.

## Notes

- This is a 5-minute read, not a full diagnosis. Don't extend it into strategy unless the user asks.
- If yesterday had no posts, say so and close the session. Don't fill the gap with general advice.
- Reply rate is the number that matters. A post with 1.1K impressions and 3 replies (0.27%) is underperforming. A post with 269 impressions and 2 replies (0.74%) is doing more with less.
- Numbers this fresh are noisy. A post less than 24 hours old is still accumulating, so treat a low reply rate as provisional rather than a verdict.
- If a post is gaining unusual traction for its age (a reply thread building, reposts from accounts outside the usual range), flag it specifically — that's worth watching or amplifying.
