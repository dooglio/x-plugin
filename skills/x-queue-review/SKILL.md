---
name: x-queue-review
description: Review the upcoming TweetHunter post queue using Claude in Chrome. Use this skill when the user wants to check their scheduled posts, audit queue quality, flag weak reply triggers, or review posts before a posting week starts. Triggers on phrases like "review my queue," "check my TweetHunter queue," "audit my scheduled posts," "what's in my queue," or any request to look at upcoming scheduled content.
---

# X Queue Review

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

You are reviewing the user's upcoming scheduled posts in TweetHunter using Claude in Chrome. Run this once a week before the posting week starts. This is a flag-only session — don't rewrite anything unless the user asks.

## Setup

Open TweetHunter in Chrome and navigate to the queue view. Read the next 7 days of scheduled posts in order.

## What to flag for each post

For each post, check four things:

1. **Reply trigger** — Is there a specific question that invites a reply? A vague CTA ("thoughts?" / "agree?") does not count. Flag if missing or weak.

2. **On-niche** — Does this post fit the niche defined in the Project Instructions? Flag if it looks off-niche or like it's chasing a trend that doesn't connect to the account's content vector.

3. **Block risk** — Does this post contain a strong opinion, culture-war angle, or anything likely to attract hostile engagement from the wrong audience? Flag if yes, with a one-line explanation of why.

4. **Queue duplication** — Is this post too similar to another post already in the queue — same topic, same structure, or same angle? Flag if yes, and name the other post it duplicates.

## Output format

Number every flag so the user can address them one at a time.

Example:
```
Post 3 (scheduled Tuesday 9am): Flag 1 — No reply trigger. The post ends with a statement, not a question.
Post 5 (scheduled Wednesday 2pm): Flag 2 — Off-niche. This is a general productivity tip with no connection to [niche].
Post 7 (scheduled Thursday 11am): Flag 3 — Duplicates Post 2. Same "I used to think X, now I think Y" structure on the same topic.
```

When you've flagged all issues, tell the user the total flag count and ask: "Ready to work through these one at a time?"

## Notes

- Don't rewrite posts in this session. Flagging is the job. If the user asks for a rewrite on a flagged post, handle it, but don't offer unsolicited rewrites.
- If TweetHunter isn't loading or the queue is empty, say so directly.
- If a post has no flags, skip it. Only report problems.
- A clean queue (zero flags) is a valid outcome. Say so if that's what you find.
