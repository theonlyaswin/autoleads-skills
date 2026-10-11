---
name: write-proposal
description: Write a client proposal / quote / statement of work for a freelancer, with clear scope, out-of-scope, milestones, 2–3 priced options and payment terms, plus a short cover message to send it. Use when the user wants a proposal, quote, estimate, pitch, SOW or pricing options for a lead or client, or to answer an Upwork-style job post. Reads autoleads/clients/<client>/client.json and autoleads/profile.json, writes proposal.md.
---

# Write Proposal

**Skill version: `1.0.0`**

Turn what the client said into a proposal they can say yes to. Free: no Autoleads credits are used.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "write-proposal"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Load inputs

- **Client**: `autoleads/clients/<slug>/client.json` (from `prep-discovery-call`) if it exists. Otherwise take whatever the user gives you: call notes, email thread, job post, a lead from the newest `autoleads/runs/*/filtered_leads.json`. Create `client.json` with the same shape `prep-discovery-call` uses.
- **Sender**: `autoleads/profile.json` — name, services, proof points, portfolio.
- **Pricing** — ask once if not already stored in `profile.json` → `pricing`:
  - how they charge (fixed project, day rate, hourly, monthly retainer),
  - their rate or minimum project size,
  - currency,
  - default payment terms (default: 50% upfront, 50% on delivery; net 7 for invoices).
  Save the answers to `profile.json` → `pricing` so you never ask again.

**Never invent prices.** Every number must come from the user's rates, their explicit instruction, or simple arithmetic on them (rate × estimated days). Show the arithmetic to the user in chat, not in the proposal.

If the scope is too vague to price, list the 2–3 questions that block pricing and ask them before writing.

## 2. Write proposal.md

Follow `references/proposal-structure.md`. Write `autoleads/clients/<slug>/proposal.md` (if one exists, write `proposal-v2.md` etc. and keep the old one).

Default to **three options** (Essential / Recommended / Complete) unless the user wants a single price or the job post asked for one fixed quote. The middle option is the one that fully solves the stated problem.

Self-check before saving:
- The first section restates the client's problem and goal in their own words — not the user's credentials.
- Every deliverable is concrete and checkable ("5-page Webflow site with CMS blog", not "website design").
- There is an explicit **Not included** list.
- Each milestone has a deliverable and a date or week number.
- Prices, currency, payment terms, revision rounds and the proposal's validity date are stated.
- One clear next step, and nothing the user didn't say they can deliver.
- No buzzwords, no padding; under ~2 pages.

## 3. Cover message

Write a 60–100 word message to send with the proposal (email or DM, matching how the client has been talking). Reference one thing from the call, say which option you recommend and why in one sentence, and give the next step. Save it at the end of `proposal.md` under `## Cover message`.

For an Upwork / job-board answer instead of a full proposal: write a 120–180 word reply that opens with their problem, gives a 3-step plan, one proof point, a price or range only if the user provided one, and a question that invites a reply.

## 4. Save and present

- Update `client.json`: `stage: "proposal_sent"`, `proposal: { "file": "proposal.md", "options": [{ "name": "", "price": 0, "currency": "" }], "valid_until": "", "sent_at": null }`.
- Show the option table and the cover message inline, plus the file path.
- Offer: shorter version, single-price version, export to HTML/PDF, or tone change. When the user changes anything, update the file.

When the client accepts an option, record it in `client.json` → `agreed_scope` (option name, deliverables, not-included list, price, milestones, revisions). `handle-scope-change` and `invoice-and-chase` read it.

## Rules

- Never invent prices, past clients, results, testimonials or guarantees.
- Never send anything yourself; the user sends the proposal.
- Don't promise dates the user hasn't confirmed they can meet.
