# Tennis Match Data MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-tennis-match-data/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-tennis-match-data) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-tennis-match-data%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-tennis-match-data&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-tennis-match-data)](https://www.npmjs.com/package/@mambalabsdev/mcp-tennis-match-data) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-tennis-match-data)](https://www.npmjs.com/package/@mambalabsdev/mcp-tennis-match-data) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-tennis-match-data)](https://github.com/mambalabsdev/mcp-tennis-match-data/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-tennis-match-data)

An MCP server for the Mamba Labs Tennis Match Data actor on Apify. ATP and WTA results, schedules, and rankings by date.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give a date or date range. The actor reads the public ESPN tennis scoreboard and returns one row per match with players, seeds, round, status, and set scores. It can also return the current ATP and WTA rankings. Refused reads and summary rows are free.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-tennis-match-data": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-tennis-match-data"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_tennis_match_data` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Get the ATP and WTA results from yesterday."
- "Show every Jannik Sinner match in July 2025."
- "Get the top 50 WTA rankings."

## Tool

`get_tennis_match_data`. One tool. Example arguments:

```json
{
  "date_from": "2025-07-13",
  "tours": [
    "atp"
  ],
  "draws": [
    "singles"
  ],
  "max_matches": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `mode` | string | no | matches (default) reads results and schedules by date. rankings returns the current ATP and WTA lists. |
| `date_from` | string | no | First UTC date as YYYY-MM-DD, or today, yesterday, or tomorrow. Needed in matches mode. |
| `date_to` | string | no | Last UTC date. Empty means the same as date from. A run reads at most 366 days. |
| `tours` | array | no | atp, wta, or both (default both). Mixed doubles appears under either. |
| `draws` | array | no | mens-singles, womens-singles, mens-doubles, womens-doubles, mixed-doubles, or the shortcuts singles and doubles. Empty means all. |
| `statuses` | array | no | final, retired, walkover, in_progress, scheduled, other, or the shortcuts finished, live, and upcoming. Empty means all. |
| `tournaments` | array | no | Keep tournaments whose name holds one of these words, such as wimbledon. |
| `players` | array | no | Keep matches with a player whose name holds one of these words, such as sinner. |
| `include_qualifying` | string | no | yes (default) or no. |
| `whole_tournament` | string | no | no (default) keeps matches played on the dates asked for. yes returns every match of each tournament running on those dates. |
| `max_matches` | number | no | The most match rows in a run. Default 500. |
| `top_n` | number | no | Rankings mode: rows per tour, up to 150. Default 100. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `match-row`, one match | $0.0018 | $0.00171 | $0.00162 | $0.00153 |
| `ranking-row`, one ranked player | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `empty-read`, one date and tour with no fitting match, after the first 2 free | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.006 | $0.006 | $0.006 | $0.006 |

Summary rows, refused reads, refused input, and the first 2 empty reads are free. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/tennis-match-data](https://apify.com/mambalabs/tennis-match-data)

## License

MIT. See [LICENSE](LICENSE).
