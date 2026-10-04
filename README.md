# Autoleads Skills

Agent skills for [Autoleads](autoleads.space) — find people on LinkedIn, X and Reddit who are asking to hire for what you do, then write personalised DMs, all from your own AI agent.

| Skill | What it does |
|---|---|
| `get-leads` | Builds intent search queries × keywords from your skills, services and ICP, runs them through the Autoleads MCP server, filters and ranks the real buyers, saves `raw_leads.json` + `filtered_leads.json` |
| `make-personalised-dms` | Writes a short, personalised DM (and follow-up) for each saved lead into `dms.json` + `dms.md` |

Searching costs Autoleads credits, which come with a monthly plan and refill on renewal. Filtering and DM writing run in your agent and are free.

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

Copy both skill folders into your agent's skills directory. For Claude Code that is `.claude/skills/` (project) or `~/.claude/skills/` (user); Codex, Antigravity and other agents that support the `SKILL.md` format each have their own skills folder — check the agent's docs.

```bash
git clone <this repo> autoleads-skills
cp -r autoleads-skills/get-leads autoleads-skills/make-personalised-dms ~/.claude/skills/
```

## 3. Use

> get me leads — I build Shopify stores for DTC brands

> write DMs for the hot leads

Output lands in your workspace:

```
autoleads/
  profile.json
  runs/2026-10-03_1430/
    raw_leads.json
    filtered_leads.json
    dms.json
    dms.md
```

Out of credits? The agent links you to the Plan page to subscribe or upgrade your monthly plan.

## Updates

Each skill states its version in `SKILL.md` and sends it on every Autoleads call. When a newer version is released the agent stops with *"new version of skill available - please update to the latest version to use"* — pull this repo and copy the skill folders again.

Releasing a new version: bump the version in the skill's `SKILL.md` (both places), push, then on the backend run
`python scripts/set_skill_version.py <skill-name> <version>`.
