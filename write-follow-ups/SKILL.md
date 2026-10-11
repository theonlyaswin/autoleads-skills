---
name: write-follow-ups
description: Write follow-up messages for leads and clients who went quiet, and show who is due a follow-up today. Covers DMs with no reply, calls with no next step, proposals with no answer, and past clients worth checking in with. Each follow-up adds something new instead of "just bumping this". Use when the user wants follow-ups, nudges, a reply to ghosting, a check-in with an old client, or asks who they should follow up with. Reads autoleads/runs/*/filtered_leads.json, dms.json and autoleads/clients/*/client.json, writes follow-ups.md.
---

# Write Follow-ups

**Skill version: `1.0.0`**

Most replies come after the second or third touch, and most freelancers stop after the first. This skill keeps track of who is due, and writes a short follow-up that gives the person a new reason to reply. Free: no Autoleads credits are used.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "write-follow-ups"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Who is due?

If the user names a person or pastes a thread, follow up with just that person (go to step 2).

Otherwise build the due list from the workspace. Today's date decides what's due; use the cadence in `references/follow-up-playbook.md`.

| Situation | Where it lives | Counts as sent when |
|---|---|---|
| **Lead DM, no reply** | `autoleads/runs/*/filtered_leads.json` leads with `dm_status: "sent"`, message in that run's `dms.json` | `dm_sent_at` |
| **After a call, no next step** | `autoleads/clients/*/client.json` with `stage: "call_done"` | `call.date` |
| **Proposal, no answer** | `client.json` with `stage: "proposal_sent"` | `proposal.sent_at` |
| **Past client check-in** | `client.json` with `stage: "complete"` | last of `paid_at` / `updated_at` |

Skip anyone with `follow_ups_paused: true`, a `replied` / `won` / `lost` status, or who already got the last step of their sequence (mark those `"no_reply"` / `"lost"` only if the user agrees).

Lead DMs need the user to say they were sent. If DMs are `drafted` but there is no `dm_sent_at`, ask once: "Which of these did you send, and roughly when?" Save the answer (`dm_status: "sent"`, `dm_sent_at`) before continuing.

Show the due list first, grouped by situation, with days since the last touch and which follow-up number it would be:

```
Due today: 6
Lead DMs (4)      Jane Doe, Acme · LinkedIn · 4 days · follow-up 1 of 3
Proposals (1)     Northwind · sent 6 days ago · follow-up 1 of 3
Past clients (1)  Bluebird Café · finished 92 days ago · check-in
```

Ask whether to write all of them or a subset. Default: all.

## 2. Write each follow-up

Follow `references/follow-up-playbook.md`. For each person, load everything known: their original post or need (`snippet`, `need_summary`), the message the user already sent, earlier follow-ups, call notes or proposal, and the user's `autoleads/profile.json` (services, `proof_points`, `testimonials`, `dm_preferences`).

Every follow-up must:
- add one new thing: a relevant idea for their project, a short example or proof point, a useful resource, a question that's easy to answer, or a deadline that's actually real,
- be shorter than the message before it (DM follow-ups: 25–60 words; X under 200 characters),
- stay in the same channel and language as the original thread,
- never guilt-trip ("just checking if you saw my message", "bumping this", "?"),
- never invent facts about them, fake urgency, discounts or availability the user didn't give.

The last step of a sequence is a polite close-the-loop message that makes it easy to say "not now". Never write a fourth chase after it.

## 3. Save

Write `autoleads/follow-ups.md` (overwrite each run), one section per person: name, situation, follow-up number, link to the post/profile/thread, the message in a quote block, and the date the next one would be due.

Record each written follow-up on its source:
- leads: in `filtered_leads.json` add to `follow_ups`: `{ "n": 1, "drafted_at": "", "sent_at": null, "message": "" }`
- clients: in `client.json` add the same shape to `follow_ups`.

When the user says they sent them, set `sent_at`. When someone replies, set the lead's `dm_status: "replied"` (or update the client's `stage`) and offer the matching next skill: `prep-discovery-call` for a lead who wants to talk, `write-proposal` if they asked for a quote.

## 4. Present

Show the first 3 messages inline, the file path, and when the next batch is due. Offer to rewrite any message (shorter, warmer, different angle).

## Rules

- Never send messages yourself; the user sends every follow-up.
- Never follow up with someone who said no, asked not to be contacted, or already replied; respect `follow_ups_paused`.
- Never invent results, offers, discounts, deadlines or details about the person.
- Max 3 follow-ups per lead or proposal, then stop.
