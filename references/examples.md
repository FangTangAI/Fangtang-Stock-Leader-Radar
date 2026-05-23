# Fangtang Examples

These examples show how to map user requests to Fangtang workflows. They do
not contain fabricated market conclusions. Fill actual conclusions only after
querying the routed sources.

## Example 1: Single-Stock Analysis

User request:

```text
分析一下 300750 宁德时代，现在是不是主线龙头候选。
```

Use:

- Workflow: `Single-Stock Analysis`
- Template: `Single-Stock Analysis`
- Budget mode: `Standard`
- Core route:
  - Resolve code and market.
  - Establish market environment.
  - Establish battery/new-energy/related sector context with TdxQuant/TdxClaw
    and RPS where available.
  - Query stock RPS through `tdx-rps-query`.
  - Use TdxQuant/TdxClaw for price-volume, K-line, and finance fields.
- Enhancement route:
  - Use Tonghuashun or Eastmoney Miaoxiang only for a material story, event,
    report, or filing gap.
  - Use Wind if normalized finance or research context is needed.

Stop early if:

- Sector strength is weak and the user did not ask for deep research.
- Price-volume and RPS do not support a leader-candidate role.

## Example 2: Multi-Stock Comparison

User request:

```text
比较一下 机器人方向的三只股票 A、B、C，谁更像龙头。
```

Use:

- Workflow: `Multi-Stock Comparison`
- Template: `Multi-Stock Comparison`
- Budget mode: `Standard`
- Core route:
  - Normalize all codes.
  - Confirm whether all names map to the same robot theme or different
    sub-themes.
  - Establish market context once.
  - Compare sector/theme strength first if candidates are not in the same
    main line.
  - Query all stock RPS with one `tdx-rps-query` batch when available.
  - Use one shared K-line window and finance period.
- Enhancement route:
  - Use story or research sources only if rank order remains unresolved after
    core data.

Stop early if:

- One candidate clearly lacks liquidity, RPS, or sector fit.
- The rank order is clear from sector role, RPS, and price-volume evidence.

## Example 3: Full-Market Screening

User request:

```text
按照 Fangtang Radar 筛一下当前 A 股强主线和前排候选。
```

Use:

- Workflow: `Full-Market Screening`
- Template: `Whole-Market Or Stock-Pool Screening`
- Budget mode: `Saver`
- Core route:
  - Use TdxQuant/TdxClaw for market environment and market ecology.
  - Use `tdx-rps-query` for industry RPS where available.
  - Find candidate sectors first.
  - Screen stocks only inside the stronger sectors.
- Enhancement route:
  - Do not use Tonghuashun or Eastmoney Miaoxiang stock-by-stock before the
    shortlist.
  - Use one or two sector/theme checks only if they clarify main-line identity.

Stop early if:

- No sector has price strength and breadth.
- Batch source data is stale or insufficient for reliable candidate ranking.

## Example 4: Sector Or Theme Review

User request:

```text
复盘一下 AI 算力板块，现在是启动、确认还是分化？
```

Use:

- Workflow: `Sector Or Theme Review`
- Template: `Sector Or Theme Review`
- Budget mode: `Standard`
- Core route:
  - Resolve classification: industry, concept, theme, or user-defined basket.
  - Establish market backdrop.
  - Check sector price strength, breadth, turnover, and RPS/proxy.
  - Identify internal roles: leader, core anchor, front row, catch-up, weak
    mapping.
- Enhancement route:
  - Use Tonghuashun for A-share theme language if installed and budget allows.
  - Use Eastmoney Miaoxiang or Wind for industry/research context when needed.

Stop early if:

- The sector is not price-strong and lacks breadth.
- The classification is ambiguous and constituent sets cannot be reconciled.

## Example 5: Follow-Up Refresh

User request:

```text
只补一下刚才那只股票的公告和故事面，不用重跑全部分析。
```

Use:

- Workflow: `Follow-Up Or Refresh`
- Template: `Candidate Verification Refresh` or `Data Gap Note`
- Budget mode: `Standard`, but with a narrow query scope.
- Core route:
  - Reuse the prior stock, date, market, and sector context.
  - Query only announcements, events, or story evidence needed for the gap.

Stop early if:

- The requested gap is resolved.
- The source is unavailable and a reliable proxy does not exist.

## Example Output Header

Use a compact header before the task body:

```markdown
## 0. Task And Data Scope

- Task type: Single-stock analysis
- Analysis universe: 300750.SZ
- Query mode: Post-market
- Market data date: <date>
- Financial reporting date: <report period>
- Source route: TdxQuant/TdxClaw + tdx-rps-query + focused enhancement if used
- Quota-limited sources used: None / Tonghuashun / Eastmoney Miaoxiang
- High-impact gaps: <gaps>
```
