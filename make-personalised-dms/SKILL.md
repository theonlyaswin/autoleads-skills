---
name: make-personalised-dms
description: Write short, personalised DMs / cold messages for leads saved by the get-leads skill, tailored to LinkedIn, X or Reddit. Use when the user wants outreach messages, DMs, cold emails or replies for their Autoleads leads. Reads autoleads/runs/<run>/filtered_leads.json and the user's autoleads/profile.json, writes one message per lead to dms.json and dms.md.
---

# Make Personalised DMs

**Skill version: `1.1.0`**

Turn saved Autoleads leads into ready-to-send messages. Everything runs in this agent — no Autoleads credits are used.

## 0. Version check

Call the `autoleads` MCP tool `check_skill_version` with `skill: "make-personalised-dms"` and `skill_version: "1.1.0"`.
- `status: "ok"` → continue.
- `status: "update_required"` → show the user its `message` exactly (`new version of skill available - please update to the latest version to use`) plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- `status: "unauthorized"` → tell the user to connect the Autoleads MCP server with their token from the Agents page, and stop.

## 1. Load inputs

- **Leads**: the latest `autoleads/runs/*/filtered_leads.json` (newest folder), unless the user names a run or a file. If none exists, tell the user to run `get-leads` first and stop.
- **Sender profile**: `autoleads/profile.json`. If it lacks a name, a one-line pitch, or a proof point (portfolio link, past client, result), ask for them once and save them back.
- **Scope**: by default all `hot` and `warm` leads with `dm_status: "pending"`. Ask before including `possible` leads. Respect any filter the user gives ("only US", "top 10").

Optional preferences to ask once (and store in `profile.json` under `dm_preferences`): tone (default: professional, friendly), channel (default: LinkedIn DM), CTA (default: soft invite to chat), length (default: under 120 words).

## 2. Write each message

Follow `references/dm-guidelines.md`, including the rules for the lead's `channel`. For every lead use **only** facts from that lead's post (`snippet`, `need_summary`, `skills_needed`, `industry`, `budget_signal`) and the sender's profile. Never invent details about the lead.

Self-check every message before saving:
- First line references something specific from their post.
- One or two sentences connect their need to what the sender does, with one concrete proof point if available.
- Ends with a soft, low-pressure CTA.
- Under the length limit, no emojis, no buzzwords, no fake familiarity.

## 3. Save

In the same run folder write:

`dms.json`
```json
{
  "generated_at": "ISO-8601",
  "source": "filtered_leads.json",
  "messages": [
    {
      "source_id": "li-…",
      "name": "",
      "profile_url": "",
      "post_url": "",
      "tier": "hot",
      "channel": "linkedin | x | reddit",
      "delivery": "dm | reply | chat",
      "message": "",
      "follow_up": ""
    }
  ]
}
```

`dms.md` — human-friendly version, one section per lead: name, tier, post link, profile link, the message in a quote block, and the follow-up.

Then set `dm_status: "drafted"` for those leads in `filtered_leads.json`.

## 4. Present

Show the first 3 messages inline and the file paths. Offer to rewrite any message (shorter, different tone, different angle) — when the user asks, update both files.

## Rules

- Never send messages yourself — the user sends them.
- Never fabricate results, clients or numbers for the sender. If there is no proof point, leave it out.
- If a lead's post has contact details, include an email version in `follow_up` only when the user asked for email.
