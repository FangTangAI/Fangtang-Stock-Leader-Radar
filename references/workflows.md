# Fangtang Workflows

Use these workflow cards to decide what to query, what to skip, when to narrow
the candidate set, and when to stop expanding data collection.

The source priorities still come from `data-routing.md`. The output shape still
comes from `task-templates.md`.

## Shared Execution Rules

1. Identify task type and universe before any data query.
2. Establish market and sector context before stock-level judgment.
3. Use batch sources first for large universes.
4. Use `tdx-rps-query` for relative strength when available.
5. Use TdxQuant first for data it can provide.
6. Use quota-limited Tonghuashun and Eastmoney Miaoxiang sources only after the
   candidate set is narrow enough, unless the user explicitly asks for deep
   research.
7. Stop expanding when additional data would only add narrative color and would
   not change market state, sector strength, stock role, candidate layer, or
   confidence.

## Full-Market Screening

Use when the user asks to find current A-share leaders without providing a
stock pool.

### Default route

1. Market pass:
   - TdxQuant or TdxClaw index and market ecology data.
   - `tdx-rps-query` industry RPS if available.
   - Do not call quota-limited stock-by-stock sources.
2. Sector pass:
   - Identify 3-8 candidate sectors or themes with price strength, breadth,
     turnover, and RPS/proxy evidence.
   - Separate industry strength from concept/story heat.
3. Candidate pass:
   - Use TdxQuant/TdxClaw batch screening and stock RPS over names inside the
     strongest sectors.
   - Keep the first candidate pool small enough to inspect, usually the top
     names by strength, liquidity, and sector role.
4. Shortlist pass:
   - Run detailed checks only on shortlisted names.
   - Use quota-limited sources for theme, event, report, or announcement gaps
     only on the shortlist.
5. Output:
   - Use the whole-market/stock-pool template.

### Stop-expansion rules

- Stop before story enrichment if no sector shows price strength and breadth.
- Stop after sector pass if candidate sectors are only rotation or abnormal
  moves and the user did not ask for speculative tracking.
- Stop after candidate pass when RPS, liquidity, or trend data is stale or
  missing for too many names; report the data gap.
- Do not expand to all stocks in a theme with quota-limited calls.

## Stock-Pool Screening

Use when the user provides a list, a CSV/XLSX pool, or a known sector component
pool.

### Default route

1. Normalize codes and names.
2. Identify whether the pool is one theme, one industry, or mixed.
3. Use TdxQuant/TdxClaw for trading state, liquidity, K-line structure, and
   available batch fields.
4. Use `tdx-rps-query` over the whole pool.
5. Rank into:
   - detailed-analysis shortlist,
   - observation names,
   - theme tracking,
   - removed or not expanded.
6. Use quota-limited sources only on the detailed-analysis shortlist.
7. Output:
   - Use the whole-market/stock-pool template.

### Stop-expansion rules

- Stop detailed analysis for names with weak liquidity, invalid trading state,
  missing core行情, or clearly weak RPS unless the user explicitly asks.
- If the pool is mixed, compare sector strength before comparing stock quality.
- If the pool has many names, cap story-side enhancement to the highest-impact
  shortlist.

## Single-Stock Analysis

Use when the user provides one stock.

### Default route

1. Resolve stock code and market.
2. Establish current market environment.
3. Identify sector, industry, concept, or theme context.
4. Judge the stock role inside that context.
5. Collect detailed evidence:
   - fundamentals,
   - story,
   - momentum,
   - capital.
6. Use quota-limited sources in standard mode if they resolve a material story,
   filing, research, or theme gap.
7. Output:
   - Use the single-stock template.

### Stop-expansion rules

- Do not skip market and sector context even if the user asks only about one
  stock.
- Stop story expansion when there is no price-volume or sector support.
- Stop fundamental deepening when the requested task is short-term leadership
  and the available financial evidence is enough to identify hard gaps.
- Use a small data-gap note instead of a full report when the user asks only to
  refresh one missing field.

## Multi-Stock Comparison

Use when the user provides two or more stocks and wants relative judgment.

### Default route

1. Normalize all names and codes.
2. Determine whether this is:
   - same-sector comparison,
   - same-theme comparison,
   - cross-theme comparison,
   - mixed watchlist ranking.
3. Establish market context once.
4. Establish sector/theme strength for each group.
5. Use one common market date, RPS口径, K-line window, and finance period.
6. Build a comparison table first.
7. Add detailed notes only where candidates are close or where one stock has a
   material gap.
8. Output:
   - Use the multi-stock comparison template.

### Stop-expansion rules

- If candidates are not in the same main line, do not rank them only by stock
  momentum. Rank sector strength first.
- Do not query reports, news, or diagnoses for every name unless the comparison
  remains unresolved after core data.
- Stop when the rank order is clear and remaining data would only make the
  explanation longer.

## Sector Or Theme Review

Use when the user asks about a board, industry, concept, policy theme, or
industry-chain direction.

### Default route

1. Resolve classification:
   - Tongdaxin industry,
   - Wind/申万/中信 industry,
   - concept/theme,
   - event-driven basket,
   - user-defined basket.
2. Establish market backdrop.
3. Establish sector price strength, breadth, turnover, and RPS/proxy.
4. Identify stage:
   - start,
   - confirm,
   - spread,
   - differentiate,
   - fade.
5. Identify internal roles:
   - leader,
   - core or mid-cap anchor,
   - front row,
   - catch-up,
   - abnormal or weak mapping.
6. Use quota-limited theme or research sources only for focused catalyst and
   story verification.
7. Output:
   - Use the sector/theme review template.

### Stop-expansion rules

- Stop before candidate deep dives if the sector is not price-strong or lacks
  breadth.
- If the classification differs across Tongdaxin, Wind, and Tonghuashun, name
  the classification used and avoid mixing constituent sets silently.
- Do not label a one-stock move as a main line without sector structure.

## Follow-Up Or Refresh

Use when the user asks for a small update, missing field, or one candidate
verification.

### Default route

1. Reuse the prior task context if available.
2. Query only the missing field or changed date.
3. State what changed and what did not.
4. Output:
   - Use the small follow-up blocks in `task-templates.md`.

### Stop-expansion rules

- Do not rerun the full workflow unless the user asks for a full refresh.
- If a source is unavailable, report the attempted source and use a proxy only
  if it is clearly labeled.

## Quota Modes

| Mode | Default use | Allowed expansion |
|---|---|---|
| Saver | Full-market and large-pool screening | Only sector-level or final-shortlist checks |
| Standard | Single stock, small comparison, sector review | Focused theme, news, report, filing, or research checks |
| Deep | Explicit user request for deep research | More source coverage, but still avoid duplicate searches |

If the mode is unclear, use saver for screening and standard for analysis.
