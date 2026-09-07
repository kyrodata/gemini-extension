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

## What you get — 14 read-only tools

| Tool | What it answers |
| --- | --- |
| `kyrodata_compare_periods` | Resolves an equal-weight comparison window (like-for-like) for the trade data, anchored on the last fully published month. |
| `kyrodata_compare_trade` | Compares Brazil's exports or imports between two equal-weight windows (like-for-like), in value (USD FOB) and in volume (kg). |
| `kyrodata_explain_pyramid_level` | Explains the pyramid's arithmetic for one commodity and horizon: the seven levels side by side (label, push %, weight share, confidence, contribution %) and, for the levels that did not enter, the reason with its ruler (hit rate vs base rate, number of origins). |
| `kyrodata_fetch` | Opens one public foreign-trade document by the id that kyrodata_search returned: an HS heading (`heading:1201`) or a partner country (`country:160`). |
| `kyrodata_get_balance` | Reports the credit balance and the limits of the API key making the request: credits left in the current cycle and when it resets, the daily credit and daily call ceilings of the key, and which tools the key reaches. |
| `kyrodata_get_climate_reading` | Current climate reading for a commodity: risk level for Brazil, a macro-region or a state (UF), the measured production shock in % of the harvest, and the projected physical loss in tonnes per horizon (1, 3, 6 and 12 months) with its range. |
| `kyrodata_get_data_coverage` | Returns what trade data exists: first and last published month (YYYYMM), whether the current year is partial, the last fully closed month, and when the aggregates were last refreshed. |
| `kyrodata_get_heading_overview` | Overview of one HS heading (SH4, 4 digits) for exports or imports: totals of the latest published year (USD FOB, kg), last closed month vs the previous one (average price per kg and volume) and a monthly price-by-volume series. |
| `kyrodata_get_hub_summary` | Reads the Kyrodata pyramid verdict for a commodity hub: direction of the leading horizon, expected move in % per horizon (1, 3, 6 and 12 months), the 80% band as a half-width in percentage points, the reason a horizon carries no arrow, which levels drive the verdict and the measured accuracy. |
| `kyrodata_get_supply_demand_balance` | Published supply and demand balance (physical, in tonnes) for one of nine agricultural hubs, season by season: production, imports, exports, consumption, initial and final stock, whether the season is still an estimate, and how many months a partial season measures. |
| `kyrodata_list_trade_partners` | Ranks Brazil's partner countries for exports or imports, optionally filtered by HS codes, over a month window (YYYYMM; the default is the last 12 published months). |
| `kyrodata_resolve_entity` | Resolves a free-text name into platform identifiers: commodity hubs (slug plus anchor SH4), HS headings (SH4), NCM codes (8 digits) and partner countries (country id). |
| `kyrodata_run_report` | Runs one of the product's pre-built reports (comex trade flows, cost surveys, hub rankings, climate history…) by id and returns its rows for the caller's plan, up to 50. |
| `kyrodata_search` | Searches the public Brazilian foreign-trade catalog and returns document ids for kyrodata_fetch: HS headings (SH4, 4 digits) and partner countries. |

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
