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

## Browsing rules (hard rules)

X breaks naive automation. Obey these or the data will be wrong:

- **Connect once:** `list_connected_browsers` → `select_browser` → `tabs_context_mcp` with `createIfEmpty: true`. Capture the `tabId` and pass it explicitly to every subsequent browser call.
- **Never scroll with the computer tool.** Use `javascript_tool`: find the real scroll container (a `div` where `scrollHeight > clientHeight * 1.2` and `clientHeight > 300` — never `document.body` on X), and drive it with `container.scrollTop += container.clientHeight * 0.8`, waiting ~450ms between steps, max 4–6 steps per `javascript_tool` call.
- **Read content via `javascript_tool` / `innerText`, never visually.**
- **X lists are virtualized:** only viewport-visible rows exist in the DOM. Accumulate `innerText` at each scroll step and dedupe with a `Set` keyed on the first 60 characters of each row. Corrupted or impossible numbers mean a row split across a virtualization boundary — re-scroll to reassemble it.

## Analytics reference

The user's handle is in the ACCOUNT CONTEXT section of the Project Instructions. Never assume a handle.

Content tab, 90-day window:

```
https://x.com/i/account_analytics/content?type=all&sort=impressions&dir=desc&days=90
```

- Swap `days=` to 7 or 30 for a shorter window.
- `sort=date` is the correct parameter for date sorting (not `sort=time`).
- If the page is already loaded on a different sort, the URL parameter alone will **not** re-sort it. Click the column header instead:

```js
const dateSpan = [...document.querySelectorAll('span')]
  .find(el => el.textContent?.trim() === 'Date');
dateSpan?.parentElement?.click();
```

- Row shape when read as `innerText`: line 3 = date, middle lines = post text, last 4 lines = Impressions / Likes / Replies / Reposts. Handle `K`/`M` suffixes when parsing.
- Analytics pages do **not** expose post status links in the DOM. Don't try to extract post IDs here.

## Workflow

1. Connect to Chrome and navigate to the content analytics URL above, 90-day window. Use 30 days if the account posts heavily; drop to a shorter window only if the user asks.

2. Scroll and scrape the full window per the browsing rules. Parse every row into: date, post text, impressions, likes, replies, reposts.

3. Compute reply rate for every post: replies ÷ impressions × 100, rounded to two decimal places.

4. **Rank by reply rate, not impressions**, and take the top 10. This is the step people get wrong. The high-impression list and the high-reply-rate list are different lists, and the second one is the one that predicts what to write next.

5. For each of the top 10, also note whether the post ends with a specific reply trigger. A direct question counts; a vague CTA like "thoughts?" does not.

6. Hold all of this in context. Don't summarize early.

## Interpretation checks

Run these before writing the brief:

**Niche vs. off-niche split.** Categorize the top 10 by whether they're niche-consistent. If more than half the high-impression posts are off-niche, the content calendar has drifted. Name the drift specifically — not "some posts are off-niche" but which ones and in what direction.

**Flag reach without resonance.** Any post with more than 3x the account's average impressions but a reply rate below 0.2% is a warning sign. Note it, and do not recommend more content like it no matter how good the impressions look.

**Growth quality check.** If follower count grew meaningfully in the period, identify which posts drove it and whether they were niche-consistent. Off-niche follower spikes are a liability, not a win. Flag them.

**Consistency over spikes.** A steady weekly reply rate beats one outlier. If the period shows one spike and everything else flat, say so plainly. The goal is inertia, not lightning in a bottle.

## Output format

**Top 10 Posts (by reply rate)**
List all 10 with: impressions, likes, replies, reply rate, trigger present (yes/no)

**Pattern: Top 5**
What do the highest-performing posts have in common? Look for: trigger type, post structure, topic, length, specificity of detail.

**Pattern: Bottom 5**
What's consistently missing? Be specific — not "less engaging" but "no trigger," "generic opener," "off-niche topic."

**Period rating**
Exactly one of:
- **Healthy niche growth** — reply rate trending up, top posts niche-consistent, follower quality solid
- **Reach without resonance** — impressions up, replies flat or declining, off-niche content dominating
- **Mixed** — some healthy signal, some noise, with specific callouts on both sides

**Single highest-leverage change**
One sentence. The most important thing to fix right now based on the data.

## Notes

- Use reply rate, not like count, to rank performance. A post with 50K impressions and 8 replies is underperforming. A post with 3K impressions and 12 replies is doing something right.
- If analytics data is incomplete or the dashboard isn't loading correctly, tell the user what you can and can't see rather than guessing. A partial audit labeled as partial is useful; a confident audit built on half the data is not.
- Don't make recommendations beyond the single highest-leverage change unless the user asks. The brief is enough for one session.
- Memory is manual. Offer to save findings at the end; don't write anything until the user says yes.
