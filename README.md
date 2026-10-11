# Autoleads Skills

Agent skills for [Autoleads](autoleads.space) — find people on LinkedIn, X and Reddit who are asking to hire for what you do, then write personalised DMs, prep the call, send the proposal, guard the scope, get paid and turn the work into your next case study, and polish the LinkedIn profile your leads click on, all from your own AI agent.

| Skill | What it does |
|---|---|
| `get-leads` | Builds intent search queries × keywords from your skills, services and ICP, runs them through the Autoleads MCP server, filters and ranks the real buyers, saves `raw_leads.json` + `filtered_leads.json` |
| `make-personalised-dms` | Writes a short, personalised DM (and follow-up) for each saved lead into `dms.json` + `dms.md` |
| `prep-discovery-call` | Free. Call-prep sheet for a lead (snapshot, agenda, must-ask questions, red flags, close), then turns your call notes into `client.json` |
| `write-proposal` | Free. Proposal / quote with scope, not-included list, milestones, 2–3 priced options from your own rates, terms and a cover message |
| `handle-scope-change` | Free. Checks a client's new request against the agreed scope, drafts a friendly reply and a priced change order |
| `invoice-and-chase` | Free. Numbered invoices (Markdown + print-to-PDF HTML), tracks paid / overdue, drafts polite escalating payment reminders |
| `write-follow-ups` | Free. Shows who's due a follow-up today (unanswered DMs, quiet calls, proposals, past clients) and writes short follow-ups that each add something new, max 3 per person |
| `write-case-study` | Free. Turns a finished project into a case study, LinkedIn / X post, testimonial request and a proof point your DMs and proposals reuse |
| `linkedin-optimizer` | Free. Paste screenshots of your LinkedIn profile; scores every section against a research-backed rubric, lists the top fixes and writes a new headline, About, banner brief and Featured plan |

Searching costs Autoleads credits, which come with a monthly plan and refill on renewal. Filtering, DM writing and every other skill run in your agent and are free. The free skills work even without the MCP server connected.

## 1. Connect the MCP server

Sign in at the Autoleads webapp, open **Agents**, and copy your MCP token and the ready-made config for your client.

Claude Code:
```bash
claude mcp add --transport http autoleads <SERVER_URL> --header "Authorization: Bearer <TOKEN>"
```

Codex (`~/.codex/config.toml`):
```toml
[mcp_servers.autoleads]
url = "<SERVER_URL>"
http_headers = { Authorization = "Bearer <TOKEN>" }
tool_timeout_sec = 600
```

Antigravity (`mcp_config.json`):
```json
{ "mcpServers": { "autoleads": { "serverUrl": "<SERVER_URL>", "headers": { "Authorization": "Bearer <TOKEN>" } } } }
```

Searches run live on LinkedIn, X and Reddit and can take a minute or two — keep the tool timeout high.

## 2. Install the skills

Copy every skill folder into your agent's skills directory. For Claude Code that is `.claude/skills/` (project) or `~/.claude/skills/` (user); Codex, Antigravity and other agents that support the `SKILL.md` format each have their own skills folder — check the agent's docs.

```bash
git clone <this repo> autoleads-skills
cd autoleads-skills && cp -r get-leads make-personalised-dms prep-discovery-call write-proposal handle-scope-change invoice-and-chase write-case-study write-follow-ups linkedin-optimizer ~/.claude/skills/
```

## 3. Use

> get me leads — I build Shopify stores for DTC brands

> write DMs for the hot leads

> prep me for my call with Jane from Acme tomorrow

> write a proposal for Acme from these call notes

> Acme wants a wishlist page too, is that in scope?

> invoice Acme for milestone 1 / who owes me money?

> who do I need to follow up with today?

> write a case study from the Acme project

> audit my LinkedIn profile (paste screenshots)

Output lands in your workspace:

```
autoleads/
  profile.json
  runs/2026-10-03_1430/
    raw_leads.json
    filtered_leads.json
    dms.json
    dms.md
  clients/acme-coffee/
    client.json
    call-prep.md
    proposal.md
    change-orders/CO-001.md
  invoices.json
  invoices/INV-2026-001.md + .html
  case-studies/acme-coffee.md
  follow-ups.md
  linkedin/audit-2026-10-11.md
```

Out of credits? The agent links you to the Plan page to subscribe or upgrade your monthly plan.

## Updates

Each skill states its version in `SKILL.md` and sends it on every Autoleads call (free skills only check when the MCP server is connected). When a newer version is released the agent stops with *"new version of skill available - please update to the latest version to use"* — pull this repo and copy the skill folders again.

Releasing a new version: bump the version in the skill's `SKILL.md` (both places), push, then on the backend run
`python scripts/set_skill_version.py <skill-name> <version>`.
