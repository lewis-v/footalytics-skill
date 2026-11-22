# Installing the Footalytics MCP Server

Footalytics is a **remote** MCP server — there is nothing to clone, build, or run locally. Installation is a single config entry pointing at the hosted endpoint.

- **Endpoint:** `https://footalytics.farvor.com/mcp`
- **Transport:** Streamable HTTP
- **Discovery:** public — `initialize` and `tools/list` work with no credential.
- **Tool calls:** require a credential (see Authentication below). Each successful tool call costs 1 credit; a free starter allowance is granted on sign-up.

## 1. Add the server to the client config

Generic MCP client config (e.g. `mcpServers` in most clients):

```json
{
  "mcpServers": {
    "footalytics": {
      "url": "https://footalytics.farvor.com/mcp"
    }
  }
}
```

Cline (`cline_mcp_settings.json`) — note the `"type": "streamableHttp"` field; without it Cline treats the entry as a local stdio server:

```json
{
  "mcpServers": {
    "footalytics": {
      "url": "https://footalytics.farvor.com/mcp",
      "type": "streamableHttp",
      "headers": {
        "Authorization": "Bearer <YOUR_API_KEY>"
      },
      "disabled": false
    }
  }
}
```

## 2. Authentication

Tool calls must send a credential as a Bearer token:

- **API key** — starts with `fxk_`. Get one by signing up at https://footalytics.farvor.com (a free starter allowance is granted on sign-up), then copy the key from your account's API settings.
- **OAuth token** — the server also supports OAuth sign-in where the client supports it; the resulting access token is sent the same way.

Without a credential the server still connects and lists its tools, but tool calls return an authentication error.

## 3. Verify the installation

Reload the client, confirm the `footalytics` server shows as connected, and call `get_account_credits` or `list_fixtures`. A normal JSON response means the setup is complete.

## Tools provided

| Tool | What it does |
| --- | --- |
| `list_fixtures` | List upcoming football fixtures (by league and/or date window). |
| `get_match_prediction` | Model prediction for one match: 1X2 probabilities, over/under 2.5, BTTS, and the most likely scoreline. |
| `list_results` | Settled results with the model's graded predictions — the public accuracy ledger. |
| `get_team` | A team's ratings, recent form, and upcoming fixtures. |
| `get_account_credits` | The signed-in account's remaining credit balance. |

## Notes

- Predictions are probabilities from a statistical model, published with a public graded track record at https://footalytics.farvor.com/accuracy — they are not betting advice.
