---
name: x-post-draft
description: Draft an X post from raw material in the user's voice. Use this skill when the user has a photo, a story, a voice note transcription, a thing that happened, or any raw input they want turned into a post. Triggers on phrases like "draft this," "turn this into a post," "write a post about," "here's the raw material," "I want to post about this," or any request to produce a post from source material.
---

# X Post Draft

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

The user has raw material they want turned into a post. Your job is to produce one draft in their voice, not a menu of options.

## Before you start

If the user hasn't provided any raw material, ask for it before doing anything else:

> "Please share your raw material — a photo, caption, voice note transcription, story notes, or anything you want to turn into a post — and I'll draft it in your voice."

Don't proceed until you have something to work from.

## What you need

The user will provide raw material: a photo caption, a transcribed voice note, a story in notes form, a thing that happened, a statistic they found. Whatever they give you is enough to start. If something critical is missing, ask after you draft — not before.

## The content formula

Every draft follows this structure:

1. **Lead with the most specific real detail or scene.** Not "I learned something recently." Not "Here's a thought." The most concrete, specific, visual thing in the raw material goes first. If there's a named place, a number, a date, a person — that's your opener.

2. **Include at least one concrete number or named place** if the material supports it. Specificity is what separates a post that feels real from one that feels like content.

3. **End with one direct reply trigger.** A specific question. Not a vague CTA. See the voice examples in the Project Instructions for how the user ends their best posts.

## Voice

Read the voice examples and format rules in the Project Instructions before writing. The draft should sound like the user wrote it at their best, not like an AI summarized their idea.

Things to watch for:
- Short sentences that land. Vary length — but the important beats hit short.
- First person throughout. "My" not "one's." "I found" not "it was found."
- No guru-speak. If a sentence could appear on a motivational poster, cut it.
- No em dashes. Use parentheses for asides or restructure the sentence.

## Output format

One draft. Then two flags:

**Flag 1 — Read-it-aloud test:** Name anything in the draft that sounds like written content rather than spoken thought. Awkward constructions, over-formal word choices, anything that would feel weird to say out loud.

**Flag 2 — Missing specificity:** Name any detail you needed but didn't have. If you invented something to fill a gap, say so — the user needs to know what to confirm or replace.

That's it. Don't offer alternatives unless the user asks. One draft, two flags, done.

## Notes

- If the raw material is thin, make the most of what's there. A short, specific post is better than a padded one.
- Thread format: only go to thread if the raw material genuinely can't fit in a single post. Default to single post.
- If the user gives you a photo (not just a caption), describe what you see in it before drafting, so they can confirm you're working from the right details.
