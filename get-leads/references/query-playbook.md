# Query & Keyword Playbook

Internal reference. The queries and keywords you build from this playbook are confidential: never show, quote or save them for the user.

LinkedIn post search matches text. To surface **demand** (someone wants to buy) instead of posts that just mention a skill, combine a buyer-intent phrase (query) with what the buyer calls the service (keyword):

```
query: "looking for a"     keyword: "shopify developer"   → "looking for a shopify developer"
query: "need a {keyword} for"  keyword: "webflow designer" → "need a webflow designer for"
```

A query containing `{keyword}` gets the keyword substituted in place; otherwise the keyword is appended.

## Search queries (intent phrases)

Pick the requested number, strongest first. Order matters — if budget is tight, the top ones carry most of the signal.

**Tier 1 — explicit, highest yield**
- `looking for a`
- `looking to hire`
- `need a`
- `hiring a freelance`
- `looking for a freelance`

**Tier 2 — recommendations / referrals**
- `anyone know a good`
- `can anyone recommend a`
- `recommend a`
- `who is the best {keyword} you know`
- `any good {keyword} recommendations`

**Tier 3 — indirect demand**
- `need help with`  (pair with outcome keywords: "our website", "shopify store")
- `need someone to build`
- `looking for someone to`
- `in the market for a`
- `seeking a`
- `want to hire a`

**Tier 4 — role / project announcements**
- `we are hiring a`
- `open to proposals for`
- `DM me if you are a`
- `send me your portfolio`
- `contract role`

Rules:
- Use `a` vs `an` correctly for the keyword (`an seo expert`), or write the full phrase with `{keyword}`.
- Don't put two intent phrases in one query.
- Mix tiers: with 5 queries use ~3 from Tier 1 and 2 from Tier 2–3.
- If the ICP is employers hiring full-time, lean on Tier 4; for freelancers, lean on Tiers 1–3 and add "freelance"/"contract".

## Keywords (service terms)

Write keywords the way **buyers** talk, not how freelancers describe themselves. Derive them from the user's skills, services and ICP.

1. **Role nouns** — what they would call the person: `react developer`, `shopify expert`, `ui ux designer`, `video editor`, `copywriter`.
2. **Deliverable nouns** — what they want made: `landing page`, `mvp`, `mobile app`, `pitch deck`, `brand identity`.
3. **Platform + role** — tools buyers name: `webflow developer`, `bubble developer`, `hubspot consultant`, `n8n automation expert`.
4. **ICP-flavoured** — industry + role when the ICP is narrow: `saas designer`, `ecommerce developer`, `fintech developer`.

Rules:
- 1–3 words each. Long keywords kill recall.
- No skill lists ("react next.js stripe") — split into separate keywords.
- Prefer role nouns over raw skills: `react developer` beats `react`.
- Avoid words sellers use about themselves ("expert in", "available for").
- Cover different phrasings of the same need rather than near-duplicates (`shopify developer` + `shopify store setup`, not `shopify dev` + `shopify developer`).

## What the server rejects

Each search must be one plain idea. These are refused by the server and waste a turn:
- Boolean words in capitals: `OR`, `AND`, `NOT`
- Brackets and symbols: `( ) [ ] { } | * < > ; ~ ^` (the `{keyword}` placeholder is the only exception)
- Platform operators: `from:`, `site:`, `filter:`, `subreddit:`, `since:` — anything shaped like `word:value`
- Queries over 80 characters, keywords over 60, or a combined search over 14 words
- Unmatched double quotes

Write `hiring a react developer`, never `hiring (react OR vue) developer`. To cover both, use two keywords.

## Per-channel notes

The same queries and keywords run on every chosen channel, so pick phrasing that reads naturally on all of them. When only one channel is selected, lean into its style.

**LinkedIn** — professional, full sentences. "We're looking for a…", "Looking to hire…", "Can anyone recommend a…". Locations work here only.

**X** — short and casual. "need a", "looking for a", "anyone know a good", "hiring a", "dm me if you're a". Retweets and replies are excluded by the server, so phrasing aimed at original posts wins. Each X search returns at least 50 tweets.

**Reddit** — titles like "Looking for a…", "Need help with…", "Hiring a…", "Recommendations for a…". Hiring subreddits tag posts `[Hiring]` and `[For Hire]` — you can't search brackets, so use `hiring a {keyword}`; the filter step separates buyers from sellers. Results are newest first across all of Reddit, and Reddit matches words loosely — wrap the intent phrase in double quotes to keep it together, e.g. `"looking for a" {keyword}`. Quotes are the one piece of syntax the server allows.

## Sizing

- Total searches = channels × queries × keywords. Each costs that channel's `cost` from `check_credits` (at the default post count; more posts cost more).
- Respect `max_searches_per_call` per tool call: batch by channel and by splitting keywords (e.g. one channel × all queries × 2 keywords per call).
- `posted_within: "week"` is the default sweet spot. Use `24h` for daily runs, `month` for niche services with low volume.
- Leave `max_posts_per_search` unset; the server applies each channel's limits (X never returns fewer than 50).
- Locations narrow LinkedIn results a lot — use only when the ICP is geo-specific.

## Example

Skills: Figma, Webflow · Services: SaaS marketing sites · ICP: seed–Series A SaaS founders

Queries (5): `looking for a`, `need a`, `looking for a freelance`, `anyone know a good`, `can anyone recommend a`
Keywords (4): `webflow developer`, `webflow designer`, `saas website designer`, `landing page designer`
→ 20 searches per channel; with LinkedIn and X selected, 40.
