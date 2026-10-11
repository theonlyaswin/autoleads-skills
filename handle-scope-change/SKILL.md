---
name: handle-scope-change
description: Stop scope creep. Check a client's new request against the agreed scope, decide if it's included, a small goodwill favour, or a paid change, and draft a friendly reply plus a change order with price and timeline impact. Use when a client asks for "one more thing", extra revisions, new features or pages, a changed deadline, or the user asks whether something is in scope. Reads autoleads/clients/<client>/client.json, writes change-orders/.
---

# Handle Scope Change

**Skill version: `1.0.0`**

Help the user say yes to more work without doing it for free, and without souring the relationship. Free: no Autoleads credits are used.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "handle-scope-change"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Load inputs

- **The request**: the client's message, pasted by the user.
- **Agreed scope**: `autoleads/clients/<slug>/client.json` → `agreed_scope`, else the accepted option in `proposal.md`, else a contract or SOW the user points to. If none exists, ask the user to paste or describe what was agreed (deliverables, revision rounds, price, deadline). Without an agreed scope, say so plainly and help the user write one down first.
- **Change log**: earlier entries in `client.json` → `changes` (count of favours already given matters).
- **Rates**: `autoleads/profile.json` → `pricing`. Ask once if missing; never invent a price.

## 2. Classify

Split the request into individual asks. For each, decide:

| Class | When | Default response |
|---|---|---|
| `in_scope` | Clearly covered by a listed deliverable or remaining revision round | Confirm and schedule it. |
| `clarification` | Ambiguous: the scope could reasonably be read either way | Agree on an interpretation in writing; lean to the client if small. |
| `favour` | Outside scope, under ~1 hour, and no favour given yet on this project | Do it once, but say it's outside scope so it's not expected next time. |
| `change` | Outside scope and bigger, or a repeat favour, or it moves the deadline | Quote it as a change order before starting. |
| `new_project` | A different goal altogether (new site, new product, ongoing work) | Suggest a separate proposal (`write-proposal`) or a retainer. |

Quote the exact line of the agreed scope (or the "Not included" list) that supports each decision. If revisions are used up, any further round is a `change`.

Estimate effort for `change` items in hours or days with a short reason, then price it with the user's rate. Show the arithmetic to the user in chat. State the deadline impact (e.g. "+3 working days").

Show the classification table to the user and let them override before drafting (some users want to absorb small changes for a good client).

## 3. Draft the reply

Follow `references/scope-replies.md`. One message to the client that:
- opens positively about the idea (never "that's not in scope" as the first words),
- states what's included and what isn't, referencing the agreement neutrally,
- gives clear options: add it now for <price> and <time impact>, swap it for something of similar size, or park it for phase 2,
- ends with one simple question ("Want me to add it?").

Under 150 words, friendly, no legalese, no apology for charging.

## 4. Change order

For every accepted or proposed `change`, write `autoleads/clients/<slug>/change-orders/CO-<NNN>.md`:

```markdown
# Change order CO-001 — <Project>
Date: <date> · Client: <client> · Status: proposed

## Change
<what's being added or changed, concretely>

## Why it's a change
Not covered by: "<quoted line from agreed scope>"

## Impact
- Price: <cur> X (<hours> × <rate>, or fixed)
- Timeline: +N working days; new delivery date <date>
- Affects: <milestones or deliverables touched>

## Approval
Reply "approved" to this message to add it to the project. Billed with <next invoice / on approval>.
```

Append to `client.json` → `changes`: `{ "id": "CO-001", "summary": "", "class": "change", "price": 0, "days": 0, "status": "proposed", "date": "" }`. Log `favour` items too (price 0) so the second favour is caught. When the user says the client approved, set `status: "approved"`; `invoice-and-chase` bills approved changes.

## Rules

- Never invent prices, rates or contract terms. Quote the agreement; don't paraphrase it into something stricter.
- Never send anything yourself.
- If the agreement is genuinely ambiguous, say so and recommend the generous reading for small items.
