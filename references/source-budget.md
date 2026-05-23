# Fangtang Source Budget

Use this file before calling quota-limited sources such as Tonghuashun skills,
Eastmoney Miaoxiang skills, Wind search/reference tools, or public search-like
fallbacks.

The goal is not to spend the fewest possible calls. The goal is to spend calls
only where they can change a Fangtang conclusion: market state, main-line
strength, stock role, candidate layer, or confidence.

## Source Classes

| Source class | Default budget behavior |
|---|---|
| Core batch sources | Use first. TdxQuant, TdxClaw batch tools, and `tdx-rps-query` should carry screening work. |
| Professional structured sources | Use when they add normalized or missing fields. Wind structured data belongs here. |
| Quota-limited smart sources | Use after narrowing. Tonghuashun and Eastmoney Miaoxiang belong here. |
| Search/reference sources | Use only for a focused missing question. Wind web/search, report search, public news, and similar sources belong here. |
| Filing/fact sources | Use when a disclosed fact matters. Announcements and company filings are worth spending calls on when they verify a key catalyst. |

## Budget Modes

| Mode | Default trigger | Call shape |
|---|---|---|
| Saver | Whole-market screening, large pools, daily review | Avoid stock-by-stock enhancement. Use 0-2 focused enhancement checks after narrowing. |
| Standard | Single stock, small comparison, sector review | Use a small number of focused enhancement checks for missing high-value evidence. |
| Deep | User explicitly asks for deep research | Expand source coverage, but avoid duplicate searches and stop when the conclusion stabilizes. |

If the user does not specify depth, use saver for screening and standard for
analysis.

## Task Budgets

| Task | Core sources | Enhancement budget |
|---|---|---|
| Full-market screening | TdxQuant/TdxClaw batch data and RPS | No stock-by-stock enhancement before shortlist. After shortlist, verify only top candidates or top sectors. |
| Stock-pool screening over more than 20 names | TdxQuant/TdxClaw batch data and RPS over the full pool | Use enhancement only on the detailed-analysis shortlist. |
| Stock-pool screening over 20 or fewer names | TdxQuant/TdxClaw batch data and RPS | Enhancement can be used for close calls or top candidates. |
| Single-stock analysis | TdxQuant/TdxClaw, RPS, sector context | Standard mode. Use focused theme, filing, report, or research checks where they fill a gap. |
| Multi-stock comparison | One shared market/RPS/finance口径 | Enhance only where rank order or role confidence remains unresolved. |
| Sector or theme review | Sector strength, RPS/proxy, breadth, turnover | Use theme/hotspot/research checks for catalyst and story verification. |
| Follow-up refresh | Reuse prior context | Query only the missing field or changed date. |

## Entry Conditions For Quota-Limited Sources

Use Tonghuashun or Eastmoney Miaoxiang only when at least one is true:

1. A candidate or sector is already shortlisted.
2. The missing evidence affects story grade, candidate layer, or confidence.
3. The user explicitly asks for Wencai, Tonghuashun, Eastmoney Miaoxiang,
   hotspot, report, announcement, diagnosis, or deep research.
4. A single sector-level query can explain several candidates.
5. A filing, announcement, or research query is needed to verify a material
   catalyst.

Do not use them when:

1. The universe is still unfiltered.
2. Core price, liquidity, or RPS evidence is missing and should be obtained
   from batch sources first.
3. The result would only make the text more detailed without changing the
   conclusion.
4. The same question has already been answered for the same stock and date.

## Deduplication Rules

- Do not ask Tonghuashun and Eastmoney Miaoxiang the same broad question in the
  same pass unless source disagreement itself is important.
- Prefer one well-scoped query over multiple broad queries.
- Query sector context before individual stocks when one sector answer can
  explain many candidates.
- Reuse prior task context for follow-up checks.
- If a result is low-confidence because of source limits, report that instead
  of spending more calls automatically.

## Stop Rules

Stop spending enhancement calls when:

- Market or sector evidence already blocks an A/B-layer conclusion.
- RPS, liquidity, or trend clearly removes a candidate from detailed analysis.
- Story evidence is enough to grade the story and further searches would only
  add examples.
- Report/search sources disagree but filings or raw market data resolve the
  material fact.
- The user asked for screening, not deep research.

## Output Disclosure

When quota-limited or search/reference sources are used, disclose:

- which source class was used,
- what gap it was meant to resolve,
- whether it changed the conclusion,
- what important gaps remain.
