---
name: x-reply-trigger
description: Generate reply trigger options for an X post. Use this skill when the user has a drafted post that's missing a reply trigger, has a weak CTA, or wants options before finalizing. Triggers on phrases like "add a reply trigger," "this post needs a trigger," "weak CTA," "give me trigger options," "how do I end this post," or any request to improve the ending of a post to drive replies.
---

# X Reply Trigger Brainstorm

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

The user has a post that needs a better reply trigger. Your job is to suggest three specific options, not rewrite the post.

## Before you start

If the user hasn't pasted a draft post, ask for it before doing anything else:

> "Please paste your draft post and I'll suggest three reply trigger options for it."

Don't proceed until you have the post text.

## What a reply trigger actually is

A reply trigger is a specific question at the end of a post that makes a particular person feel like they have something to say. "What do you think?" is not a reply trigger — it's a non-question. A reply trigger names the reader's situation or experience and asks them to confirm, contrast, or add to it.

Good: "What's the one thing you wish you'd known before your first month in [specific situation]?"
Bad: "Have you experienced this? Drop a comment."

## What you need from the user

The user will paste the post text. Read it, then read the voice and niche from the Project Instructions. The trigger options have to fit both the post and the voice — not just be generically "engaging."

## Output format

Produce exactly three options:

**Option 1 — Personal experience question**
A question that asks the reader to share something from their own history or situation that connects directly to what the post just said.

**Option 2 — Local knowledge or opinion question**
A question that draws on something the reader knows from their specific context — their market, their city, their industry, their niche — that the post can't answer for them.

**Option 3 — Before/after or decision question**
A question about a choice the reader made or is facing, where their answer reveals something about where they are in the same journey the post describes.

For each option, write one sentence explaining why it fits this specific post. Not why reply triggers work in general — why this one fits this post.

Then stop. The user will pick one. Don't add a fourth option, don't rank them, don't recommend a winner unless asked.

## Notes

- Read the voice examples in the Project Instructions before writing options. The trigger has to sound like the user, not like a social media coach.
- Shorter is almost always better. A trigger that runs longer than one sentence usually loses the reader.
- If the post already has a trigger but the user thinks it's weak, name what's weak about it before presenting the alternatives.
