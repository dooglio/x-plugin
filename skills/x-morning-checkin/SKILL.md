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

## Workflow

1. Navigate to `https://analytics.twitter.com/user/BowTiedYanqui/tweets` in Chrome.

2. Click the **Posts** dropdown (top left, below the tab bar) and select **Posts and Replies**.

3. Click the **calendar icon** (top right of the date controls, next to the 7D/2W/4W/3M buttons).

4. In the calendar picker, select yesterday's date as both the start and end date. Yesterday is one day before today's date. Set both the start and end of the range to yesterday so only that single day's posts appear.

5. Read the table. For each post visible, record:
   - Post text (first ~10 words is enough to identify it)
   - Impressions
   - Likes
   - Replies
   - Reposts

6. Calculate reply rate for each: replies ÷ impressions × 100, rounded to two decimal places.

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
- If the calendar picker won't select a single day (some X UI versions require a range), set start = yesterday and end = yesterday.
- If yesterday had no posts, say so and close the session. Don't fill the gap with general advice.
- Reply rate is the number that matters. A post with 1.1K impressions and 3 replies (0.27%) is underperforming. A post with 269 impressions and 2 replies (0.74%) is doing more with less.
- If a post is gaining unusual traction for its age (e.g., a reply thread building, reposts from accounts outside the usual range), flag it specifically — that's worth watching or amplifying.
