---
name: x-account-audit
description: Run a full X account audit using Claude in Chrome. Use this skill when the user wants to audit their X analytics, check post performance, review reply rates, or get a breakdown of what's working and what isn't on their account. Triggers on phrases like "run an audit," "audit my account," "check my analytics," "what's working," or any request to review X post performance data.
---

# X Account Audit

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

You are running a full account audit using Claude in Chrome connected to the user's X analytics dashboard. This is a dedicated analytics session — don't draft posts or review queues here.

## What you're looking for

The signals that matter: reply rate (replies ÷ impressions × 100), dwell time proxies (bookmarks, profile clicks), and whether reply triggers are present. Likes and impressions alone tell you almost nothing useful.

## Workflow

1. Navigate to the user's X analytics dashboard in Chrome.

2. Find the top 10 posts from the last 30 days sorted by impressions. If fewer than 10 posts exist in that window, expand to 60 or 90 days.

3. For each post, record:
   - Post text (full)
   - Impressions
   - Likes
   - Replies
   - Reply rate (replies ÷ impressions × 100, rounded to two decimal places)
   - Whether the post ends with a specific reply trigger (a direct question counts; a vague CTA like "thoughts?" does not)

4. Hold all of this in context. Don't summarize early.

## Output format

When you've finished reading all 10 posts, produce the full audit brief in this order:

**Top 10 Posts**
List all 10 with: impressions, likes, replies, reply rate, trigger present (yes/no)

**Pattern: Top 5**
What do the highest-performing posts have in common? Look for: trigger type, post structure, topic, length, specificity of detail.

**Pattern: Bottom 5**
What's consistently missing? Be specific — not "less engaging" but "no trigger," "generic opener," "off-niche topic," etc.

**Single highest-leverage change**
One sentence. The most important thing to fix right now based on the data.

## Notes

- Use the reply rate, not the like count, to rank performance. A post with 50K impressions and 8 replies is underperforming. A post with 3K impressions and 12 replies is doing something right.
- If analytics data is incomplete or the dashboard isn't loading correctly, tell the user what you can and can't see rather than guessing.
- Don't make recommendations beyond the single highest-leverage change unless the user asks. The brief is enough for one session.
