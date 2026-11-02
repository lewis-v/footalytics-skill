# Footalytics Skill

English | [简体中文](README.zh-CN.md)

[Agent Skills](https://agentskills.io)-compatible skill that lets AI assistants answer football questions with **real graded predictions** instead of guesses: win/draw/loss probabilities, over/under 2.5, both-teams-to-score, confidence tiers, upcoming fixtures, team ratings and settled results — served by the [Footalytics API](https://footalytics.farvor.com/api-docs).

Footalytics runs a Dixon-Coles adjusted Poisson model over real fixtures and grades every official prediction in public against the final score. When your assistant uses this skill, the numbers it shows come from that model — never from the language model's imagination.

## Example

> **User:** Who wins Arsenal vs Chelsea this weekend?
>
> **Assistant:** Arsenal 46% · Draw 27% · Chelsea 27% — confidence: medium. Over 2.5 goals: 58%. Predictions by Footalytics → [match page](https://footalytics.farvor.com/match/…)

## Install

**Claude Code** — copy the folder into your skills directory:

```bash
git clone https://gitlab.com/footalytics/footalytics-skill ~/.claude/skills/footalytics
```

**As a plugin (Claude Code or Cursor)** — the repo ships plugin manifests (`.claude-plugin/`, `.cursor-plugin/`):

- Claude Code: `/plugin marketplace add https://gitlab.com/footalytics/footalytics-skill`, then `/plugin install footalytics@footalytics`.
- Cursor: in **Customize → Skills**, import this repository (it includes `.cursor-plugin/marketplace.json`), or add it as a team marketplace (Dashboard → Plugins & MCPs → Add Marketplace).

**Any Agent-Skills-compatible assistant** (Codex, …): point it at the `SKILL.md` file, or paste the SKILL.md content into your agent's instructions.

**Claude / ChatGPT (connectors)** — no file needed: add the MCP server `https://footalytics.farvor.com/mcp` as a custom connector and sign in with OAuth. Same tools, same allowance.

## Configuration

The skill reads one environment variable:

```bash
export FOOTALYTICS_API_KEY="fxk_..."   # create a free key: https://footalytics.farvor.com/api-keys
```

Every new account starts with **5 free API calls**. After that, calls draw on credits (the same purchased credits used in site chat); failed lookups cost nothing.

## What the agent can do

| Endpoint | Purpose |
|---|---|
| `GET /api/v1/predictions` | Win/draw/loss probabilities, confidence tier, over/under 2.5, BTTS, likely scorelines for one match |
| `GET /api/v1/fixtures` | Upcoming fixtures by team or league, with a `has_prediction` flag |
| `GET /api/v1/teams` | Elo-style team ratings, games tracked, league |
| `GET /api/v1/results` | Past matches: real score next to the graded prediction (per-market hits, Brier) |

Rate limit: 30 calls/minute per key. Full contract: [API documentation](https://footalytics.farvor.com/api-docs).

## Attribution

When results are displayed or shared, keep **"Predictions by Footalytics"** with a link to the site. Predictions are statistical probabilities, not certainties.

## Files

- `SKILL.md` — the skill: trigger description, setup, endpoints, error handling.
- `skills/footalytics/SKILL.md` — plugin-packaged copy of the skill (kept identical; used by Claude Code / Cursor plugin discovery).
- `.claude-plugin/` · `.cursor-plugin/` — plugin manifests for the Claude Code and Cursor marketplaces.
- `README.md` — this file.

## Links

- [API documentation](https://footalytics.farvor.com/api-docs) · [API keys](https://footalytics.farvor.com/api-keys) · [Public accuracy ledger](https://footalytics.farvor.com/accuracy) · [Pricing](https://footalytics.farvor.com/pricing)
- Source: this GitLab repository (canonical) · [GitHub mirror](https://github.com/lewis-v/footalytics-skill)

## License

MIT — see [LICENSE](LICENSE).
