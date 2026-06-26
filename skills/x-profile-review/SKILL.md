---
name: x-profile-review
description: Review the user's X profile as a conversion page using Claude in Chrome. Use this skill when the user wants to assess their bio, header, pinned post, or link in bio — especially after a niche shift, when follower conversion from viral posts seems low, or when they haven't reviewed the profile in a while. Triggers on phrases like "review my profile," "check my bio," "is my pinned post good," "profile audit," "why aren't viral posts converting to followers," or any request to evaluate the X profile page.
---

# X Profile Review

## Project check

Before doing anything else, check whether you're running inside the correct Claude Project. Look for "X GROWTH OPERATOR: PROJECT INSTRUCTIONS" in your current context.

If you don't see it, stop and say:

> "This skill is designed to run inside your X Growth Claude Project, where your account context, niche, and voice instructions live. It looks like those instructions aren't loaded in this conversation. Open your X Growth project and run this skill from there."

Don't proceed until the project context is confirmed.

---

You are reviewing the user's X profile as a conversion page — the thing that turns someone who found one post into a follower. Open the live profile in Chrome and assess each element against the niche and audience defined in the Project Instructions.

## The four elements

**Bio**
Does it lead with what the reader gets, or with who the user is? A good bio answers "why should I follow this account" in the first few words. A bad bio leads with the user's credentials or job title.

**Header image**
Does it reinforce the niche visually, or is it generic (a landscape, a logo, something that tells the reader nothing about what the account is for)?

**Pinned post**
Does it represent the best of what the user posts — high reply rate, demonstrates the voice, shows what the account is about — or is it just recent? A pinned post is the second thing many profile visitors read. It should be working hard.

**Link in bio**
Is the destination appropriate for where the user is in their funnel? A lead magnet makes sense for an account that's building a list. A paid product link makes sense if the account already has an audience. A random link that doesn't connect to the niche loses people.

## Output format

For each element:
- **Current state:** What's actually there right now
- **What's working:** One thing that's doing its job (if anything)
- **What's missing:** The specific gap, not a vague direction

Then:

**Priority fix**
If the user could only address one thing this week, what is it and why? Be direct. Pick one.

## Notes

- Base the assessment on the niche and audience in the Project Instructions. A bio that works for a finance account doesn't work for a travel account.
- If any element can't be seen because the profile is private or loading fails, say so.
- Don't recommend changes to the voice or posting strategy here — this is a profile review, not an account audit. Stay on the four elements.
