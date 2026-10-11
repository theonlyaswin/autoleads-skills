---
name: linkedin-optimizer
description: Audit and optimise the user's LinkedIn profile from screenshots so the people they DM (and the people who find them) turn into clients. Asks the user to paste screenshots of their profile, scores every section (photo, banner, headline, About, Featured, Experience, Skills, Recommendations, Services, activity) against a research-backed rubric, and gives a prioritised fix list plus ready-to-paste rewrites. Use when the user wants to improve, review, audit, rewrite or optimise their LinkedIn profile, headline, About section or banner, or asks "why is nobody replying / viewing my profile".
---

# LinkedIn Optimizer

**Skill version: `1.0.0`**

Every DM the user sends gets a profile click before it gets a reply. This skill makes that click convert. Free: no Autoleads credits are used.

Tone: professional, direct, encouraging. A good consultant, not a cheerleader and not a critic. Every point names the problem, why it costs them clients, and the exact fix.

## 0. Version check

If the `autoleads` MCP server is connected, call `check_skill_version` with `skill: "linkedin-optimizer"` and `skill_version: "1.0.0"`.
- `status: "update_required"` → show the user its `message` exactly plus the `update_url`, and stop.
- `status: "account_deleted"` → show the `message` and stop.
- Anything else (`ok`, `unauthorized`, tool not available) → continue. This skill works without an Autoleads account.

## 1. Ask for the screenshots

If the user hasn't already shared them, send this (adapt lightly, keep it short):

> Paste screenshots of your LinkedIn profile, viewed on desktop if you can:
> 1. **Top card**: photo, banner, name, headline, location, buttons
> 2. **About** (click "see more" first so the full text shows)
> 3. **Featured** and **Services** (if you have them)
> 4. **Experience**: your current role and one or two past ones, expanded
> 5. **Skills** and **Recommendations**
>
> Also tell me in one line: **who do you want to hire you, and for what?** (e.g. "DTC brands on Shopify who need a faster store").

Also read `autoleads/profile.json` if it exists: services, ICP, rates and `proof_points` / `testimonials` tell you who they sell to and give you real proof to use in rewrites. If there's no target client from either source, ask for it: you can't judge a profile without knowing who it's meant to convince.

If a screenshot is unreadable or a section is missing, say which one and carry on with the rest. Mark missing sections "not seen", never guess what's in them. If the user prefers, they can paste the text of a section instead of a screenshot.

## 2. Audit

Judge every section against `references/linkedin-rubric.md`. That rubric is the only standard: don't add personal taste on top. For each section:
- Score it **0–10** using the rubric's scoring guide.
- List what works (1–2 bullets, only if true).
- List what's costing them clients, most important first, each with the concrete fix.

Then give an overall score: the weighted total from the rubric, out of 100.

Rules for the audit:
- Judge against **their target client**, not a generic job-seeker. A freelancer's profile is a sales page; "Open to work" job-seeker advice only applies if they're actually job hunting.
- Quote their exact words when pointing at a problem ("'Passionate about creating solutions' says nothing a buyer can check").
- No vague notes. "Improve your headline" is useless; "Lead with 'Shopify developer for DTC brands', the words your buyers search" is useful.
- Be honest about photos and banners but respectful. Comment on lighting, crop, framing, background, resolution and expression, never on the person's looks, body, age, race or anything they can't change in a reshoot.

## 3. Rewrite

Write ready-to-paste versions of what scored lowest, always including:
1. **Headline**: 3 options built from the rubric formula, each under 220 characters with the key phrase in the first ~60. Label the angle of each (keyword-led, result-led, niche-led).
2. **About**: a full rewrite (opening 2 lines that work before "see more", then the body, then a clear CTA). Keep their voice: if they write casually, don't make it corporate.
3. **Banner brief**: the text and layout for a 1584 × 396 banner (one value line, optional proof/CTA, safe zone clear of the photo on the left). Don't generate the image; give them the brief to build in Canva or Figma.
4. **Featured**: what 3 items to pin and in what order, using their real work.
5. Any other section scoring ≤ 5: rewrite or a specific to-do (e.g. 2 Experience bullets rewritten as outcomes; the 5 skills to pin; who to ask for a recommendation and a message to ask them).

**Never invent** clients, results, numbers, certifications, titles or testimonials. Use only what's in the screenshots, `profile.json` or what the user tells you. Where a number would help but you don't have one, write a placeholder like `[X stores launched]` and tell them to fill it in or cut it.

## 4. Save and present

Write `autoleads/linkedin/audit-<YYYY-MM-DD>.md` with:
- Score table: section, score, one-line verdict, plus the overall /100.
- **Top 3 fixes**: the three changes that will move the needle most, in order, each doable in under 15 minutes where possible.
- Full section-by-section audit.
- All rewrites from step 3.
- A short checklist the user can tick off.

If an earlier audit exists in `autoleads/linkedin/`, compare: show the old vs new scores and call out what improved.

In chat, show: the overall score, the score table, the top 3 fixes, and the 3 headline options. Point to the file for the rest. Offer next steps: "Want the About in a different tone, a shorter version, or posts to fill your Featured section? (write-case-study can turn a finished project into one.)"

If `autoleads/profile.json` exists, save `linkedin_headline` (the option the user picks, once they pick) and `linkedin_audit_score` so other skills can reference it.

## Rules

- Never log into LinkedIn, scrape it, or change the profile yourself. The user makes every edit.
- Never fabricate experience, results, endorsements or recommendations.
- Never advise buying followers, endorsements, engagement pods or fake reviews.
- Don't comment on protected characteristics or physical appearance.
- Platform limits change. Where the rubric gives a character limit, tell the user to trust what LinkedIn's editor shows if it differs.
