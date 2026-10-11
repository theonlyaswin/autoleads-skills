---
name: invoice-and-chase
description: Create clean invoices for freelance work and chase late payments politely. Generates numbered invoices (Markdown + printable HTML) from agreed scope, milestones, approved change orders or hours, tracks what's paid and overdue, and drafts escalating payment reminders. Use when the user wants to invoice a client, bill a milestone, see who owes them money, or write a payment reminder / overdue notice. Writes autoleads/invoices/ and autoleads/invoices.json.
---

# Invoice and Chase

**Skill version: `1.0.0`**

Get the user paid on time with less awkwardness. Free: no Autoleads credits are used. This skill drafts documents and messages; it never sends them or takes payments.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "invoice-and-chase"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Business details (once)

Load `autoleads/profile.json` → `billing`. If missing, ask once and save:
- legal / business name, address, email, tax ID (VAT/GST/etc., optional),
- currency, tax rate if they charge tax (default: none),
- payment methods and details exactly as they want them shown (bank, PayPal, Wise, Stripe link…),
- default payment terms (default: due in 7 days),
- late fee policy (default: none).

Never invent tax rates, tax IDs or bank details. If the user is unsure whether to charge tax, say that depends on their country and registration and leave it at 0 unless they decide.

## 2. Create an invoice

Sources, in order:
- `autoleads/clients/<slug>/client.json` → `agreed_scope` (milestones, payment split) and `changes` with `status: "approved"` not yet billed,
- hours or line items the user gives,
- anything else the user describes.

Ask for the client's billing name/address/email if not in `client.json` and save it there under `billing`.

Number invoices `INV-<YYYY>-<NNN>`, continuing from the highest number in `autoleads/invoices.json` (start at 001).

Write `autoleads/invoices/INV-YYYY-NNN.md` and `INV-YYYY-NNN.html` using `references/invoice-template.md`. The HTML is a single self-contained, print-ready page (A4/Letter, no external assets) so the user can open it and "Print → Save as PDF".

Check the maths twice: line totals, subtotal, tax, total. Show the total and due date in chat.

Record in `autoleads/invoices.json`:
```json
{
  "invoices": [
    {
      "number": "INV-2026-001",
      "client_slug": "acme-coffee",
      "client_name": "",
      "issued": "YYYY-MM-DD",
      "due": "YYYY-MM-DD",
      "currency": "USD",
      "total": 0,
      "items": [{ "description": "", "qty": 1, "unit_price": 0 }],
      "covers": ["milestone:1", "CO-001"],
      "status": "draft | sent | paid | overdue | void",
      "sent_at": null,
      "paid_at": null,
      "reminders": []
    }
  ]
}
```

Also write a 2–3 sentence message to send with the invoice (what it covers, total, due date, how to pay). Mark approved change orders as billed in `client.json`.

When the user says it's sent, set `status: "sent"` and `sent_at`. When paid, set `status: "paid"` and `paid_at`, and if it was the final invoice set the client's `stage: "complete"` and offer **"Want a case study from this project? (write-case-study)"**.

## 3. Who owes me?

When asked, or whenever the skill runs, compare today's date with every `sent` invoice's `due` date. Mark overdue ones `overdue` and show a short table: client, number, total, days overdue, last reminder.

## 4. Chase late payments

Follow `references/reminder-sequence.md`. Pick the stage from days overdue and reminders already sent, draft that message for the user's channel (email by default), and log it in the invoice's `reminders` (`{ "stage": 1, "date": "", "channel": "email" }`) once the user says it was sent.

Never threaten legal action, collections or late fees unless the user explicitly chooses to, and never state a late fee that isn't in their terms.

## Rules

- Never send invoices or messages, and never touch payment systems.
- Never invent amounts, tax rates, bank details or legal text.
- Never delete invoices; void them (`status: "void"`) and issue a new number.
