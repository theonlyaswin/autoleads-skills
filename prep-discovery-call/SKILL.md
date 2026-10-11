---
name: prep-discovery-call
description: Prepare a freelancer for a discovery / sales call with a lead or client, then capture the call notes. Use when the user has a call booked, a lead replied, or they ask for call prep, questions to ask, an agenda, or to save notes after a call. Works from an Autoleads lead (filtered_leads.json) or anything the user pastes (job post, email thread, website). Writes autoleads/clients/<client>/call-prep.md and client.json.
---

# Prep Discovery Call

**Skill version: `1.0.0`**

Get the user ready for a call with a prospect in two minutes, and turn their notes afterwards into something `write-proposal` can use. Free: no Autoleads credits are used.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "prep-discovery-call"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Find the prospect

- If the user names a lead, look it up in the newest `autoleads/runs/*/filtered_leads.json` (match on name, handle or company). Use its `snippet`, `need_summary`, `skills_needed`, `industry`, `budget_signal`, `post_url`, `profile_url`.
- Otherwise use what the user pastes: job post, DM thread, email, website text, LinkedIn about section.
- If `autoleads/clients/<slug>/client.json` already exists for this prospect, load it and build on it.
- Load `autoleads/profile.json` for the user's services, skills and proof points. If it is missing, ask for a one-line description of what they do.

`<slug>` = lowercase company or person name with hyphens, e.g. `acme-coffee`.

Ask at most one short question if something critical is missing (e.g. "Is this a 15-minute intro or a full scoping call?"). Default: 30-minute scoping call.

## 2. Write call-prep.md

Follow `references/call-framework.md`. Write `autoleads/clients/<slug>/call-prep.md`:

1. **Snapshot** — who they are, what they asked for (their words), channel and link, what's known about budget and timeline. Mark anything unknown as *unknown* — never guess.
2. **What they probably care about** — 2–3 likely underlying goals behind the request (e.g. "launch before Black Friday", "stop paying agency retainers"), each tied to evidence from their post. Label them as hypotheses.
3. **Agenda** — a timed agenda for the call length.
4. **Questions** — 8–12 questions grouped by: the problem, success, scope, timeline, budget, decision process. Mark the 4 must-ask ones with ★.
5. **Your angle** — how the user's services and proof points map to this need; one relevant example to mention.
6. **Red flags to listen for** — from the framework, only the ones relevant to this lead.
7. **Close** — the exact next step to propose at the end of the call (usually: "I'll send a proposal with 2–3 options by <day>").

Keep it to one screen: bullets, no padding.

## 3. Save client.json

Create or update `autoleads/clients/<slug>/client.json`:

```json
{
  "slug": "acme-coffee",
  "name": "",
  "company": "",
  "contact": { "email": null, "profile_url": null, "post_url": null },
  "source": { "channel": "linkedin | x | reddit | referral | other", "lead_source_id": null },
  "need_summary": "",
  "stage": "call_booked",
  "call": { "date": null, "notes": null, "budget": null, "timeline": null, "decision_maker": null, "goals": [], "requirements": [], "concerns": [] },
  "updated_at": "ISO-8601"
}
```

If the lead came from `filtered_leads.json`, set its `dm_status` to `"replied"` there.

## 4. After the call

When the user pastes or dictates notes (any shape — bullets, transcript, voice memo text), extract into `client.json` → `call`: date, budget, timeline, decision maker, goals, requirements, concerns, and keep the raw notes in `notes`. Set `stage: "call_done"`. Write nothing the notes don't support.

Then show:
- a 3-line summary of what was agreed,
- any must-ask question that went unanswered,
- the next step: **"Want me to draft the proposal? (write-proposal)"**.

## Rules

- Never invent facts about the prospect; hypotheses must be labelled as such.
- Never contact the prospect. The user runs the call and sends every message.
- Don't overwrite existing notes in `client.json`; merge and keep earlier calls in order.
