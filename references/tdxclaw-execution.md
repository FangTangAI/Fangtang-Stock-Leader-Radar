# TdxClaw Execution

Use this file when Fangtang Radar runs in a TdxClaw or Tongdaxin-heavy
environment. The framework still comes from the no-data-source reference and
the source priority still comes from `data-routing.md`.

## Execution Order

1. Use TdxQuant for covered data when the runtime can call it.
2. Use `tdx-rps-query` for stock RPS and Tongdaxin industry-board RPS.
3. Use available TdxClaw `tdx_*` tools for Tongdaxin-side行情, board, F10,
   formula, and screening work when they are the exposed platform surface.
4. Use quota-aware Tonghuashun or Eastmoney Miaoxiang skills for focused
   story, theme, report, announcement, and research gaps.
5. Use public fallback sources only for material gaps.

## Tool Map

| Need | TdxClaw surface | Notes |
|---|---|---|
| Stock and board lookup | `tdx_lookup_stock` | Resolve ambiguous names and index codes before K-line calls |
| Market ecology and natural-language Tongdaxin screening | `tdx_screener` | Useful for涨停, 跌停, 连板,板块关键词, and focused screen prompts |
| Quotes and trading state | `tdx_quotes` | Use amount, turnover, market cap, price, and change fields when exposed |
| K-lines and trend structure | `tdx_kline` | Use日线 and周线 windows consistently across compared names |
| F10, board, industry, theme, capital, holders | `tdx_api_data` | Prefer specific entries and tags for structured checks |
| Focused financial field selection | `tdx_indicator_select` | Efficient but protect against retry storms |
| Relative strength | `tdx-rps-query` | Direct RPS route; do not label return proxies as RPS |

## Framework Mapping

| Framework step | Preferred TdxClaw route |
|---|---|
| Market environment | Index K-lines, market ecology screens, industry RPS where relevant |
| Main-line sector | Sector screens, board/industry data, board K-lines, industry RPS |
| Initial stock screening | RPS batch query, quotes, K-lines, focused screen results |
| Fundamental analysis | TdxQuant-supported finance first; then `tdx_indicator_select` or structured `tdx_api_data` |
| Story analysis | Theme and event fields from Tongdaxin; quota-aware theme/news/report sources for gaps |
| Momentum analysis | Stock RPS plus daily and weekly K-line structure |
| Capital analysis | Quotes and price-volume acceptance first; structured capital, holder,龙虎榜, or两融 fields as auxiliary evidence |

## Bounded Call Batches

### Market pass

Use one market pass before candidate expansion:

```text
tdx_lookup_stock query="沪深300" range="ZS"
tdx_lookup_stock query="中证500" range="ZS"
tdx_lookup_stock query="中证1000" range="ZS"
tdx_lookup_stock query="创业板指" range="ZS"
tdx_lookup_stock query="科创50" range="ZS"

tdx_screener message="涨停"
tdx_screener message="跌停"
tdx_screener message="3连板"
tdx_screener message="主力净流入"
tdx-rps-query --scope industry --metrics HY_RPS5,HY_RPS20,HY_RPS60,HY_RPS120 --json
```

Resolve index codes before `tdx_kline`; the older TdxClaw draft notes that
codes such as中证1000 can be misread if lookup is skipped.

### Sector pass

Use sector checks after the market pass identifies a direction:

```text
tdx_screener message="<行业或题材关键词>"
tdx_kline code="<板块指数>" setcode="<market>" period="4" wantNum="60"
tdx-rps-query --scope industry --industry-names "<通达信行业名>" --metrics HY_RPS20,HY_RPS60 --json
```

Use structured board and industry `tdx_api_data` entries when the platform
exposes them for board details, industry-chain notes, stage performance, or
important industry events.

### Candidate pass

Use batch RPS before expensive candidate enrichment:

```text
tdx-rps-query --scope stock --codes <codes> --metrics RPS5,RPS20,RPS60,RPS120,RPS250 --json
tdx_quotes code="<code>" setcode="<market>"
tdx_kline code="<code>" setcode="<market>" period="4" wantNum="250"
tdx_kline code="<code>" setcode="<market>" period="5" wantNum="60"
```

Only shortlisted candidates should receive broader theme, research, holder,
capital-flow, or report enrichment.

## Useful Structured Checks

The older TdxClaw draft used these `tdx_api_data` patterns. Use them only when
the runtime still exposes the entry and the field resolves a material gap.

| Check | Example pattern |
|---|---|
| Theme labels | `entry="TdxSharePCCW.tdxf10_gg_rdtc"` with theme tag such as `zttzztk` |
| Event labels | `entry="TdxSharePCCW.tdxf10_gg_rdtc"` with event tag such as `sjcd` |
| Profit statement | `entry="TdxShareCW.ph_agf10_cw_lyb"` with report or single-quarter tag |
| Balance sheet | `entry="TdxShareCW.ph_agf10_cw_zcfzb"` |
| Cash flow | `entry="TdxShareCW.ph_agf10_cw_xjllb"` |
| Business composition | `entry="TdxShareCW.ph_agf10_jyfx"` |
| Earnings warning | `entry="TdxSharePCCW.tdxf9_ag_cwsj_yjyj"` |
| Valuation history | `entry="TdxShareCW.ph_agf10_gzfx"` |
| Industry valuation rank | `entry="TdxShareCW.ph_agf10_hypm"` |
| Capital flow, northbound, financing,龙虎榜 | trading-data entry family such as `tdxf10_gg_jyds` |
| Holder count and top holders | holder entry family such as `tdxf10_gg_gdyj` |

These examples are adapter hints, not a promise that every TdxClaw runtime
exposes every entry unchanged.

## Failure Handling

- If `tdx_indicator_select` times out or returns service errors, retry at most
  three times with increasing waits and keep concurrent requests low. Degrade
  to structured TdxQuant/TdxClaw fields or other focused sources after that.
- If a TdxClaw screen returns an approximate ecology proxy rather than a true
  full-market breadth field, label it as a proxy.
- If RPS slots are stale or unavailable, use a labeled return comparison only
  as a fallback.
- If a board name maps to multiple concepts or industries, state which
  Tongdaxin board or theme route was used.
