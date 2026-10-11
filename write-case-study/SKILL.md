---
name: write-case-study
description: Turn a finished freelance project into proof that wins the next client — a portfolio case study, a LinkedIn / X post, a short testimonial request to the client, and a one-line proof point saved to the user's Autoleads profile so future DMs and proposals can use it. Use when a project is done, the user wants a case study, portfolio piece, social post about their work, or to ask a client for a testimonial or review.
---

# Write Case Study

**Skill version: `1.0.0`**

Every finished project should make the next sale easier. Free: no Autoleads credits are used.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "write-case-study"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Gather the story

Start from `autoleads/clients/<slug>/client.json` and `proposal.md` if they exist (problem, goals, scope, timeline). Then ask the user, in one message, only for what's missing:
- What was the situation before? What was the client struggling with?
- What did you build / do? Any key decisions or obstacles?
- What changed after? **Real numbers if they have them** (conversion, revenue, time saved, launch date hit, pages shipped). "None yet" is a fine answer.
- Can the client be named publicly? (default: no, describe them anonymously, e.g. "a DTC coffee brand")
- Any quote from the client, or should we ask for one?
- Links: live site, screenshots, repo, Dribbble, etc.

**Never invent results, metrics or quotes.** If there are no numbers, use concrete, checkable outcomes ("launched in 3 weeks", "replaced 4 plugins with one custom app") instead.

## 2. Write the pieces

Follow `references/case-study-formats.md`. Write `autoleads/case-studies/<slug>.md` containing:

1. **Case study** (300–500 words): title that states the result, then Challenge → Approach → Result, a pull quote if one exists, and a "Services" line.
2. **LinkedIn post** (120–200 words): hook line, story, one lesson, soft CTA. No hashtag spam (max 3), no emojis unless the user wants them.
3. **X post** (under 280 characters), or a 3–5 post thread if the user asks.
4. **Testimonial request** (if there's no quote yet): a short message to the client that makes it easy to say yes, with 2–3 prompting questions and an offer to draft it for their approval.
5. **Proof point**: one sentence under 25 words, e.g. "Built a Recharge subscription flow for a DTC coffee brand that launched in 3 weeks."

Respect the anonymity choice everywhere: no client name, logo or identifying detail if they can't be named.

## 3. Feed the rest of Autoleads

Append the proof point to `autoleads/profile.json` → `proof_points` (array of `{ "text": "", "client_slug": "", "services": [], "link": null, "added_at": "" }`). `make-personalised-dms` and `write-proposal` use these as proof in future messages. If the user has a portfolio link for it, include it.

If the client sends a testimonial later and the user pastes it, add it to the case study and to `profile.json` → `testimonials` (`{ "quote": "", "author": "", "role": "", "public": true }`) exactly as written.

## 4. Present

Show the LinkedIn post and the proof point inline, plus the file path. Offer variations: shorter, more technical, different angle, or a version for their website.

## Rules

- Never fabricate results, quotes, client names or logos.
- Never post or send anything yourself.
- Ask before naming a client if `client.json` doesn't say they can be named.
