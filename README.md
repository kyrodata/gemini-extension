# Kyrodata — Brazilian trade, crop and commodity data for Gemini CLI

A [Gemini CLI](https://github.com/google-gemini/gemini-cli) extension that connects
[Kyrodata](https://kyrodata.com/en-US/developers) to your terminal over the
[Model Context Protocol](https://modelcontextprotocol.io).

Ask in plain language — *"how did Brazil's coffee exports do this year against last?"* —
and get the measured figure, the window it covers, and a link to the screen behind it.

```
Coffee, exports — Jan–Aug 2026 vs Jan–Aug 2025
  Value   USD 7.91 bn   (−13.6%)
  Volume  1.293 bn kg   (−9.0%)
  Source: MDIC/ComexStat
```

## Install

```bash
gemini extensions install https://github.com/kyrodata/gemini-extension
```

Gemini CLI will ask for your **Kyrodata API key** — create one at
[kyrodata.com/user/api-keys](https://kyrodata.com/user/api-keys). It is shown only once.

Prefer to wire it by hand? The server is a plain remote MCP endpoint:

```bash
gemini mcp add --transport http \
  --header "Authorization: Bearer $KYRODATA_API_KEY" \
  kyrodata https://mcp.kyrodata.com/mcp
```

## What you get — 12 read-only tools

| Tool | What it answers |
| --- | --- |
| `kyrodata_trade_compare` | Exports/imports between two equal-weight windows, in USD FOB and kg |
| `kyrodata_list_trade_partners` | Top partner countries, with growth |
| `kyrodata_get_heading_overview` | Overview of an HS heading (SH4) |
| `kyrodata_compare_periods` | Builds the like-for-like window the other tools use |
| `kyrodata_get_supply_demand_balance` | Supply and demand balance sheet |
| `kyrodata_get_climate_reading` | Climate reading and physical crop loss |
| `kyrodata_hub_summary` | Commodity hub summary and forecast verdict |
| `kyrodata_explain_pyramid_level` | Explains one level of the forecast pyramid |
| `kyrodata_run_report` | Runs one of the catalog reports |
| `kyrodata_resolve_entity` | Resolves a country, HS code or commodity |
| `kyrodata_get_data_coverage` | Coverage and the latest closed month |
| `kyrodata_get_balance` | Credit balance and limits for your key |

Every tool is **read-only** (`readOnlyHint: true`, `destructiveHint: false`). Nothing here
writes to your account or to Kyrodata.

## Comparisons are like-for-like

Every percentage compares **two windows of equal length** — month against month, quarter
against quarter, year-to-date against the same year-to-date. Three months of 2026 are never
compared against a full year of 2025, and every result carries the label of the window it
actually measured.

Value and volume move differently, and both are returned: coffee can be down 13.6% in USD
while down 9.0% in kilograms. The tool tells you which is which.

## Coverage

Brazilian foreign trade from **2000 onwards** (MDIC/ComexStat), monthly, by HS code
(SH4/SH6/NCM) and partner country — plus production, supply and demand, climate readings
and commodity forecasts for the covered hubs. "No signal" is a valid answer: the server
says so rather than inventing one.

## Credits

Most tools are free. Trade queries are metered in credits, and repeated identical
questions inside a session are not charged twice. `kyrodata_get_balance` always reports
where you stand and costs nothing.

## Other clients

The same endpoint works in Claude Code, Claude Desktop, VS Code, Cursor, Codex CLI, n8n,
Zapier, Make and Microsoft Copilot Studio — by API key, or by OAuth 2.1 where the client
supports it. Copy-paste setup for each:
[kyrodata.com/en-US/developers](https://kyrodata.com/en-US/developers).

Also listed in the [official MCP Registry](https://registry.modelcontextprotocol.io)
as `com.kyrodata/kyrodata`.

## License

MIT — see [LICENSE](LICENSE). This repository contains only the extension manifest; the
Kyrodata server itself is a hosted service.
