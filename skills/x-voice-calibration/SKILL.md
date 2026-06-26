---
name: x-voice-calibration
description: Calibrate Claude's drafting voice against the user's actual X voice. Use this skill when the user feels Claude's drafts are drifting from their voice, when they want to test how well Claude has internalized their style, or after any significant gap between drafting sessions. Triggers on phrases like "calibrate my voice," "you're not sounding like me," "voice check," "drafts don't sound right," "recalibrate," or any complaint that posts feel off-brand or generic.
---

# X Voice Calibration

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

The user wants to run a voice calibration session. Either Claude's drafts have been drifting, or this is a fresh check after a gap. The goal is to surface exactly where the voice is off so the Project Instructions can be tightened.

## Before you start

If the user hasn't provided three rough ideas, ask for them before doing anything else:

> "Please give me three rough ideas to work with — short notes, not polished. A thing that happened, a topic you've been thinking about, a photo you took. I'll draft each one as a post and then show you where my output is drifting from your actual voice."

Don't proceed until you have at least two ideas to work from.

## What you need

The user will provide three rough ideas — short notes, not polished thoughts. You'll draft each one as a post, then diagnose the gaps.

## Drafting

For each rough idea, produce one post draft following the content formula:
- Lead with the most specific real detail
- Include at least one concrete number or named place if the material supports it
- End with a direct reply trigger

Read the voice examples and format rules in the Project Instructions before drafting. The goal is to match the user's actual best posts, not a generic "good X post."

## Diagnosis

After you've drafted all three, produce a calibration report:

**Off-brand elements**
Name the specific things that feel wrong — word choices, sentence rhythms, structural habits that showed up in your drafts but don't appear in the voice examples. Be precise. "Too formal in the opener" is useful. "Doesn't sound quite right" is not.

**Closest voice example**
For each draft, name which of the voice examples from the Project Instructions it's closest to. If none of them are close, say that explicitly.

**What to update**
Based on what you caught, suggest one or two specific additions or edits to the WHAT I NEVER SAY or FORMAT RULES sections of the Project Instructions. Write them in the same format as the existing rules — specific and actionable, not directional.

## Notes

- The user tells you what to update. You suggest; they decide. Don't modify the Project Instructions directly unless asked.
- If all three drafts feel on-brand and the voice examples confirm it, say so. A clean calibration is a valid outcome.
- This session is not for fixing the rough ideas — it's for fixing the voice model. Keep that as the focus.
