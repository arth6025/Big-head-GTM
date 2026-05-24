# Weekly AI Digest — PM Task Brief
**[PM TASK — assign to product owner]**

---

## Feature Name
**Weekly AI Digest** — Your store's AI visibility, delivered every Monday morning.

One-liner: A personalized weekly email that shows each paid user exactly how their AI visibility moved, who's winning in their category, and the one thing to do this week to get more citations.

---

## Problem This Solves

### Why users churn without it
Big Head's core value is ongoing visibility tracking — but tracking only matters if users remember to look. Free users run an audit, see their score, and disappear. Paid users who don't see consistent movement lose the thread: they can't tell if their efforts are working, if competitors are pulling ahead, or if there's a specific gap to close. Without a forcing function, even genuinely engaged users stop logging in within 3–4 weeks.

The Weekly AI Digest is that forcing function. It moves the product from "tool I occasionally remember" to "Monday morning read I look forward to."

### What paid users need to stay
Paid users are paying for an edge. They need to feel that edge concretely — a number moving up, a competitor slipping, a prompt they didn't know to own. Without that weekly signal, the subscription feels passive. With it, they feel like they have an analyst working for them.

---

## What the Feature Does

Every Monday between 8–9am in the user's timezone, they receive a digest email containing five modules:

**1. Visibility Score This Week**
Your AI Visibility Score vs. last Monday. Net change shown prominently (+7 points, -3 points). Sparkline showing 4-week trend. Single sentence interpreting the movement ("You've held top-3 citations on 'best running shoes under $150' for the second week in a row.").

**2. New Citations Earned**
Breakdown of new citations detected this week across ChatGPT, Perplexity, Gemini, and Claude. Which AI engine cited you most. Which prompt queries triggered the citations. Callout if any citation is in a new category the brand hasn't owned before.

**3. Competitor Movement**
Up to 3 competitor brands tracked for this user. Who gained visibility this week and on which queries. Who lost ground. If a tracked competitor jumped significantly, flag it with context ("Allbirds gained 12 citations this week on 'sustainable sneakers' — they may have updated their FAQ or schema markup.").

**4. Top Missed Prompt Opportunity**
One high-volume prompt query where competitors are getting cited but the user's brand is not. Framed as a specific gap: the query, which competitors are winning it, and a one-line hypothesis for why. This is the weekly "itch" — it should be concrete enough that the user can act on it the same day.

**5. One Recommended Action**
A single, specific CTA for the week. Not a list. Not suggestions. One action: update a specific FAQ, add a schema block to a specific page, write a comparison paragraph addressing a specific topic. The action should map directly to the missed opportunity in module 4.

---

## The PLG Angle

This is not a retention email. It is a re-acquisition moment that happens 52 times a year.

Every Monday digest lands with four distinct audiences simultaneously:
- **Active paid users** — reinforces value, drives feature engagement
- **Dormant paid users** — pulls them back in before they cancel
- **Free users who haven't upgraded** — if they're on a free plan, the digest is a preview/teaser that shows what they're missing (redacted competitor data, locked action item → upgrade prompt)
- **Churned users** — if email permission persists, a re-engagement variant of the digest ("here's what changed since you left") can be a reactivation vehicle

The upgrade moment is organic: a free user sees a locked module ("3 competitors moved this week — unlock to see who") and the friction to upgrade is low because the value is already visible.

Key design principle: the digest should feel like it came from a smart analyst who worked through the weekend, not like a product notification.

---

## What "Good" Looks Like

| Metric | Target (Month 3) |
|---|---|
| Open rate | >45% (industry avg for SaaS digest: ~28%) |
| Click-through rate | >18% |
| Upgrade attributions from digest CTA | >10% of new paid conversions |
| Churn rate for digest-engaged users vs. not | 40% lower |
| Re-activation rate (dormant users clicking digest) | >8% monthly |

Open rate is the primary signal. If the email is genuinely useful, people open it because they want to know the answer, not because of a good subject line.

---

## Key Design Decisions the PM Must Make

**1. Segmentation: Brand view vs. Agency view**
A solo DTC brand owner wants their single store's digest. An agency managing 12 brands needs a roll-up: top movers across the portfolio, which client needs attention this week, aggregate citation growth. These are different products sharing an engine. Decision needed: do we build both in v1 or start with brand and layer agency view into a later release? Recommendation: brand-first, but build the data model to support multi-store from day one so the agency view isn't a rebuild.

**2. Competitor data sourcing**
How do we know who a user's competitors are? Options: (a) user-defined during onboarding or in settings, (b) auto-detected based on category and citation overlap, (c) a combination where we suggest and they confirm. Auto-detection is more PLG-friendly (less setup friction) but requires inference logic. Decision needed on the default set and the max number of tracked competitors per plan tier.

**3. Personalization depth**
Does every user get the same template with their data filled in, or does the recommended action adapt to their store's category, content maturity, and prior actions? Shallow personalization (fill-in-the-blank) ships faster. Deep personalization (LLM-generated commentary based on their specific store's data) is more compelling but requires prompt engineering and cost modeling per send. MVP should be shallow with the system designed to plug in deep personalization later.

**4. Cadence options**
Monday weekly is the default. Some power users will want daily; some agencies will want Friday-for-the-week summaries. Decision: lock to Monday weekly in v1, add cadence preferences in v2. Do not offer daily in v1 — it creates noise before the product has enough signal density to justify it.

---

## Dependencies

Before this feature can ship, the following must exist:

- **Citation tracking pipeline** — real-time or near-real-time detection of brand mentions across ChatGPT, Perplexity, Gemini, Claude. This is table stakes. If citation data is incomplete or delayed, the digest is unreliable and damages trust faster than not having the feature.
- **AI Visibility Score** — a stable, explainable score that moves meaningfully week-over-week. Needs to be defined and validated before it's featured prominently in an email.
- **Competitor tracking** — the product needs to be running audits on competitor stores, not just user stores. This has cost and infrastructure implications.
- **Email infrastructure** — transactional email system (Postmark, SendGrid, or similar) with per-user scheduling, timezone handling, and open/click tracking tied back to user IDs in the product DB.
- **User store data freshness** — weekly digest is meaningless if the underlying store data is stale. Automated weekly re-audit of each paid user's store needs to run before the digest goes out.

---

## Success Metrics

**Primary:**
- Weekly active users (WAU) for digest-engaged cohort vs. non-engaged
- Paid plan retention at 30/60/90 days, segmented by digest engagement
- Upgrade conversion rate attributed to digest clicks

**Secondary:**
- Module-level click rate (which of the 5 modules drives the most action)
- Time-to-first-open (Monday morning behavior formation)
- Unsubscribe rate (should stay below 0.3%)

**Qualitative:**
- User interviews at 30 days: "Does the digest change how you think about AI visibility for your store?"

---

## Suggested MVP Scope

**Build first (v1):**
- Weekly Monday email with modules 1, 2, and 5 (score, citations, one action)
- Single brand view only
- User-defined competitors (up to 3)
- Shallow personalization (template + data fill)
- Single cadence: Monday 8am local time
- Free plan teaser variant with competitor module locked

**Build later (v2+):**
- Module 3 (competitor movement) with auto-detected competitors
- Module 4 (missed prompt opportunity) with LLM-generated specificity
- Agency roll-up view
- Cadence preferences (weekly/bi-weekly)
- Deep personalization via LLM commentary
- Re-engagement variant for churned users
- A/B testing on subject lines and module order

---

*Last updated: May 2026 — Big Head product team*
