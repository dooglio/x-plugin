---
name: x-growth-diagnosis
description: Diagnose why X account growth has stalled or reply rates have dropped. Use this skill when the user's metrics have been flat or declining for two or more consecutive weeks, when engagement has dropped off, or when posting feels like it's not working anymore. Triggers on phrases like "growth has stalled," "my reply rate is dropping," "nothing is working," "run a diagnosis," "what's wrong with my account," or any request to investigate a pattern of underperformance.
---

# X Growth Diagnosis

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

The user's growth has stalled or reply rates are declining. This is a diagnosis session — not a quick fix. The goal is to identify the root cause before recommending anything.

## Before you start

If the user hasn't provided post data, ask for it before doing anything else:

> "To run a diagnosis I need your last 30 days of post data — impressions, replies, and reply rate for each post. You can export this from TweetHunter or copy it from your X analytics dashboard. Paste it here when you're ready."

Don't proceed until you have the data.

## What you need

The user will provide data for the last 30 days: a list of posts with impressions, replies, and reply rate. This can come from TweetHunter exports or copied from X analytics.

## Four diagnostic questions

Work through each one in order. Don't skip ahead.

**1. Is there a performance pattern?**
Which posts are performing (reply rate above median) vs. underperforming (reply rate below median)? Look for patterns: topic, format, post length, time of day, whether a reply trigger was present. Name the pattern specifically — not "some posts do better" but "every post without a direct question underperforms."

**2. Is there voice drift?**
Compare recent posts against the voice examples in the Project Instructions. Are they getting more generic? More formal? More like "content" and less like something a real person wrote? Voice drift is common after a few weeks of volume posting. Name what drifted and when it started if you can tell.

**3. Is there a reply trigger problem?**
What percentage of posts in the data set end with a specific reply trigger? A specific question, not a vague CTA. If it's below 70%, that's the problem. If it's above 70% but reply rates are still low, the triggers are weak — they're present but not earning replies.

**4. Is there off-niche content fragmenting the signal?**
Are any posts clearly outside the niche defined in the Project Instructions? The algorithm builds a content vector from what you post. Off-niche posts don't just underperform — they dilute the vector and make the next on-niche post work harder to reach the right audience.

## Output format

**Findings**
One paragraph per diagnostic question. State what you found, not what might be the case. If the data doesn't support a conclusion on one of the four, say so.

**Priority list**
Rank the issues from most to least impactful. Start with the one most likely to move the reply rate if fixed in the next two weeks.

**First action**
One specific thing to do this week. Not a strategy — an action. "Add a direct question to every post in the next 7 days" is an action. "Focus more on engagement" is not.

## Notes

- Don't recommend things not supported by the data. If the data is thin (fewer than 10 posts), say so and qualify your conclusions accordingly.
- If the data shows no obvious problem — good triggers, on-niche content, consistent voice — say that. Sometimes growth stalls for reasons outside the content (algorithm shifts, follower composition, external events). Acknowledge that rather than inventing a diagnosis.
- This is a one-issue-at-a-time session. Give the user one thing to fix first, then they can run this again after two weeks to see if it moved.
