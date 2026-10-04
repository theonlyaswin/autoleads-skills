# Lead Filter Rubric

Classify every raw post. Goal: keep real inbound **demand** — someone who wants to buy a service, hire a freelancer / agency / contractor, or hire an employee for work the user can do.

## Keep (buyer)

Demand is often indirect. All of these count:
- "who can build / design / make …", "anyone know a good …", "any recommendations for a …"
- "need help with …", "need someone to …", "in the market for …", "looking to build / launch …"
- "we're expanding the team", "open role for …", "now hiring …"
- a scoped problem + ask: "our site is slow, need a dev to fix it"

## Drop

- **Sellers** — people advertising their own services, portfolio or availability. The buyer/seller line is the most important call:
  - "I'm a freelance designer, DM me for work" → DROP (seller)
  - "Looking for a freelance designer, DM me" → KEEP (buyer)
- Agencies / freelancers posting "we build X for you" offers
- Job seekers ("open to work", "looking for a new role")
- News, opinions, hot takes, "X is dead" posts
- Course / product launches, newsletters, giveaways, self-promotion
- Pure discussion / advice / "how do I learn X"
- Keyword overlap only, no one wants anything
- Demand for a service the user clearly does not offer (keep it only if adjacent and plausible)

When unsure whether it is a buyer, keep it with a lower score — a dropped lead is gone.

## Channel-specific rules

**LinkedIn** — job posts and "we're hiring" announcements count, but mark `hire_type: fulltime` unless contract or freelance is stated. Recruiters posting for a client are buyers only if the role fits the user's offering.

**X** — short posts lose context; score intent only on what the tweet says. Drop tweets that are jokes, polls, threads about hiring in general, or engagement bait ("drop your portfolio below 👇" from accounts that farm replies). An author bio that describes their own freelance services usually means a seller. `can_dm: false` doesn't disqualify a lead, but note it.

**Reddit** — read the title and body together. In hiring subreddits, `[Hiring]` / `[Task]` in the title or flair = buyer; `[For Hire]` = seller, drop. Posts in r/forhire, r/hiring, r/slavelabour, r/freelance_forhire, r/jobbit are the strongest buyer signals. Posts asking how to *become* a freelancer, or venting about clients, are not demand. Low-effort `[Hiring]` posts with no scope and absurd budgets are `possible` at best.

## Extract per kept lead

- `channel` — copy `source` from the raw post (`linkedin`, `x`, `reddit`)
- `name` — post author (person or company)
- `handle` / `community` — X username or Reddit username, and the subreddit for Reddit
- `need_summary` — one line, what they want
- `snippet` — the buying-intent excerpt, verbatim from the post
- `hire_type` — `freelance` (project/contract), `fulltime` (employee), `agency` (explicitly agencies), `null`
- `skills_needed` — 3–10 short lowercase tokens (`react`, `stripe`, `video editing`)
- `industry` — 1–3 lowercase tokens (`saas`, `fintech`, `ecommerce`)
- `region` — ISO-3166 alpha-2 (`US`, `IN`, `GB`) from location, headline, currency, phone code, language; `null` if no signal
- `budget_signal` — `scoped` when budget / timeline / scope is named, `exploratory` when shopping around, `null`
- `contact` — email / phone / website only if present in the post text
- `why_fit` — one line linking their need to the user's skills/services

## Scores (0–10)

**intent_score** — how clearly they want to buy now
- 9–10 explicit hire intent right now ("hiring freelance X", "need X this week")
- 7.5–9 strong signal, asking for recommendations / quotes / DMs
- 6–7.5 warm, exploring vendors, project not scoped yet
- 5–6 plausible demand phrased loosely — keep
- <5 no real demand — drop

**relevance** — how well the need matches the user's skills, services and ICP
- 9–10 exactly what the user sells, in their ICP
- 7–9 core match, ICP partly matches
- 5–7 adjacent — the user could do it
- <5 not the user's service — drop

**Freshness / reachability adjustments** (apply to `fit_score`, not the raw scores)
- +0.5 posted in the last 48h
- +0.5 contact details or "DM me" present
- −1 post older than 3 weeks
- −1 `hire_type: fulltime` when the user only wants freelance work (unless they said otherwise)

`fit_score = round(0.6 × intent_score + 0.4 × relevance + adjustments, 1)`

## Keep threshold & tiers

Keep when `intent_score ≥ 5` **and** `relevance ≥ 5`.

| Tier | fit_score |
|---|---|
| hot | ≥ 8 |
| warm | 6.5 – 7.9 |
| possible | 5 – 6.4 |

Drop everything else. Dedupe: if the same author has several kept posts, keep the highest-scoring one and mention the others in `need_summary`.
