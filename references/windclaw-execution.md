# WindClaw Execution

Use this file when Fangtang Radar runs in a WindClaw environment. Wind enhances
the framework; it does not fork the framework or redefine the TdxQuant and RPS
priority rules.

## Execution Order

1. Use `tdx-rps-query` for stock and Tongdaxin industry relative strength when
   available.
2. Use TdxQuant first for covered market, K-line, snapshot, formula, list, and
   supported finance fields when WindClaw can reach that route.
3. Use Wind tools for normalized financial data, professional reference
   content, Wind-side行情/data access, research context, and source gaps.
4. Use quota-aware Tonghuashun or Eastmoney Miaoxiang skills when A-share theme
   language, hotspot context, or focused research enhancement is needed.
5. Use public filing or public market sources when a disclosed fact or fallback
   field is required.

## Wind Tool Map

| WindClaw surface | Best fit |
|---|---|
| `get_wind_data` | Structured行情, financial, and queryable Wind data fields |
| `wind_financial_reference_content` | Deep reference context, financial logic, company or industry research background |
| `wind_web_search` | Focused current policy, news, event, and external context |
| `document_search` | Focused research-report, announcement, or rule/document lookup |

### Wind Query Fit

| Question | Preferred Wind tool |
|---|---|
| Index, stock, board行情, turnover,涨幅, or structured market ecology | `get_wind_data` |
| Multi-period revenue, profit, ROE, margin, cash flow, or valuation comparison | `get_wind_data` |
| Company logic, competition, industry trend, or valuation framework | `wind_financial_reference_content` |
| New policy, latest event, internet news, or a current context gap | `wind_web_search` |
| Focused research report, announcement, or regulation/document lookup | `document_search` |

Useful focused prompt shapes from the older Wind draft:

```text
get_wind_data: "宁德时代 近20日涨幅 成交额 换手率 2026-05-21"
get_wind_data: "贵州茅台 2024-2025 季度营收 净利润 ROE 毛利率"
wind_financial_reference_content: "AI算力产业链 竞争格局 投资逻辑 估值框架"
wind_web_search: "人工智能 政策 最新 2026-05-20"
document_search: "宁德时代 财报 公告 doctype=3"
document_search: "AI算力 公司研究 研报 doctype=2"
```

## Framework Mapping

| Framework step | Preferred WindClaw route |
|---|---|
| Market environment | TdxQuant market route when available; otherwise Wind structured market fields |
| Main-line sector | TdxQuant and RPS strength first; Wind adds industry context and structured board comparison where available |
| Initial stock screening | Avoid search-heavy screening; use structured market fields and RPS |
| Fundamental analysis | TdxQuant-supported fields first; Wind is strong for normalized multi-period and cross-company financial comparison |
| Story analysis | Tonghuashun theme route when quota fits; Wind search/reference/document tools for focused verification and research |
| Momentum analysis | RPS plus price-volume structure; Wind market fields only when local Tongdaxin route is absent |
| Capital analysis | Price-volume and turnover first; Wind or other source-labeled flow fields remain auxiliary |

## Wind Call Discipline

- Prefer a structured Wind data call before search or reference expansion when
  the question is a field lookup.
- Use one focused query per missing question. Do not run web, document, and
  reference searches just to produce a fuller narrative.
- Follow platform call-frequency limits if WindClaw enforces them. The older
  Wind draft used a conservative pattern of at most one call per Wind tool in
  one answer and then reported remaining gaps.
- Keep market screening viable without Wind web or document search.

## Typical Passes

### Single-stock analysis

1. Resolve market, sector, and RPS route.
2. Use TdxQuant or structured Wind data for price-volume and core financial
   fields.
3. Add Wind financial reference or document search only for a material
   fundamental, research, or announcement gap.
4. Add quota-aware theme or hotspot sources only when story-side evidence is
   missing.

### Multi-stock comparison

1. Use one common date window and financial口径 across names.
2. Use RPS for relative-strength comparison when available.
3. Use Wind normalized financial comparison when the compared companies need a
   cleaner cross-company fundamental table.
4. Search reports or news only for comparison-critical differences.

### Sector or main-line review

1. Establish price strength and structure with TdxQuant/RPS or structured Wind
   market fields.
2. Use Wind research/reference context for industry background and durable
   drivers.
3. Use theme/hotspot sources for A-share market narration when the sector is a
   concept or event-driven主线 rather than a clean Wind industry bucket.

## Output Rules

- State whether the market route came from TdxQuant, Wind, or a fallback.
- State whether RPS was direct `tdx-rps-query` or unavailable.
- Separate Wind reference/search commentary from disclosed facts and raw
  market data.
- When Wind and Tongdaxin industry classifications differ, name the
  classification used before comparing candidates.
