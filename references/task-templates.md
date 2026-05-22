# Fangtang Task Templates

Use these templates to shape the answer after the framework, data contract,
data route, and platform adapter are already selected.

Templates are skeletons:

- Keep only sections that the task needs.
- Prefer the smallest template that answers the task. A short refresh should
  not be expanded into a full radar report.
- Fill tables only with fields supported by the retrieved data.
- Keep screening and analysis separate. Do not spend detailed-analysis effort
  on every unfiltered name in a large universe.
- State data gaps instead of filling blanks with invented data.

## Shared Header

Use this header for non-trivial outputs:

```markdown
## 0. Task And Data Scope

- Task type:
- Analysis universe:
- Query mode: post-market / real-time
- Market data date:
- Financial reporting date:
- Filing and event cutoff:
- Source route:
- Quota-limited sources used:
- High-impact gaps:
```

## Whole-Market Or Stock-Pool Screening

Use this for full-market screening and large or medium stock pools. The first
pass should stay batch-oriented.

```markdown
## 1. Market Environment

- Market state:
- Index and style context:
- Turnover and participation:
- Short-line ecology:
- Constraint on candidate quality:

## 2. Main-Line Sector Scan

| Rank | Sector Or Theme | Type | Price Strength | RPS Or Proxy | Turnover/Breadth | Candidate Structure | Main-Line Judgment |
|---|---|---|---|---|---|---|---|

## 3. Initial Candidate Screen

- Screening universe:
- Hard exclusions or unavailable names:
- Batch fields used:
- Screening logic:

| Rank | Code | Name | Sector/Theme | Initial Role | Liquidity/Amount | RPS | Trend/Price-Volume | Screen Result |
|---|---|---|---|---|---|---|---|---|

## 4. Shortlist Analysis

### Candidate 1: Name (Code)

- Sector role:
- Fundamental check:
- Story check:
- Momentum check:
- Capital check:
- Main gap:
- Candidate layer:

## 5. Candidate Layers

| Layer | Names | Reason |
|---|---|---|
| A leader candidates | | |
| B front-row candidates | | |
| C observation candidates | | |
| Theme tracking | | |
| Removed or not expanded | | |

## 6. Information Gaps

- High-impact gaps:
- Degraded proxies:
- Quota reserved for later verification:
```

## Single-Stock Analysis

Single-stock analysis still requires market and sector context. Step 3 is role
judgment, not a yes/no deletion screen.

```markdown
## 1. Market Environment

- Market state:
- Style context:
- Constraint on this stock:

## 2. Main-Line Sector Context

- Sector or theme:
- Main-line type:
- Main-line stage:
- Sector strength:
- Sector structure:
- Evidence and caveats:

## 3. Stock Role Judgment

- Stock:
- Trading state and liquidity:
- Role candidate: emotional leader / trend leader / core holding / front row / catch-up / abnormal move / non-main-line
- Role evidence:
- Role uncertainty:

## 4. Detailed Stock Analysis

### 4.1 Fundamentals

- Logic type:
- Revenue and profit:
- Quality and cash-flow checks:
- Valuation fit:
- Fundamental conclusion:

### 4.2 Story

- Core label:
- Business mapping:
- Catalysts and events:
- Evidence grade:
- Story conclusion:

### 4.3 Momentum

- RPS:
- Daily and weekly structure:
- Price-volume behavior:
- Strength versus sector and market:
- Momentum conclusion:

### 4.4 Capital

- Amount and turnover:
- Price acceptance:
- Source-labeled flow or holder clues:
- Capital conclusion:

## 5. Overall Judgment

- Candidate layer:
- Confidence:
- Core reasons:
- Main doubts:

## 6. Information Gaps

- High-impact gaps:
- Mixed-date or source caveats:
```

## Multi-Stock Comparison

Use one market date, one comparison universe, and one relative-strength口径
where possible.

```markdown
## 1. Market And Sector Context

- Market state:
- Compared stocks:
- Same main line or cross-main-line comparison:
- Stronger sector or theme:

## 2. Comparison Table

| Dimension | Candidate A | Candidate B | Candidate C |
|---|---|---|---|
| Sector/theme | | | |
| Sector strength | | | |
| Role | | | |
| Liquidity and amount | | | |
| RPS or proxy | | | |
| Trend and price-volume | | | |
| Fundamental fit | | | |
| Story evidence | | | |
| Capital evidence | | | |
| Key gap | | | |

## 3. Candidate Notes

### Candidate A

- Strongest evidence:
- Weakest evidence:
- Role confidence:

## 4. Layering

| Rank | Candidate | Layer | Why It Ranks Here |
|---|---|---|---|

## 5. Comparison Conclusion

- Best leader candidate:
- Best front-row or core candidate:
- Best observation candidate:
- Main uncertainty:
```

## Sector Or Theme Review

Use this for an industry, concept, event-driven theme, or industry-chain
review.

```markdown
## 1. Market Backdrop

- Market state:
- Style fit for this sector/theme:

## 2. Sector Or Theme Strength

- Name and classification used:
- Type: industry / concept / event / industry chain / style
- Strength evidence:
- RPS or proxy:
- Turnover and breadth:
- Catalyst context:
- Stage: start / confirm / spread / differentiate / fade

## 3. Internal Structure

| Role | Candidate Names | Evidence |
|---|---|---|
| Leader | | |
| Core or mid-cap anchor | | |
| Front row | | |
| Catch-up | | |
| Abnormal or weak mapping | | |

## 4. Focused Candidate Checks

| Code | Name | Role | Fundamental Fit | Story Fit | Momentum | Capital | Gap |
|---|---|---|---|---|---|---|---|

## 5. Sector Judgment

- Main-line judgment:
- Best-supported names:
- Names that need more verification:
- Classification or source caveats:
```

## Small Follow-Up Blocks

Use these when the user asks for a narrower refresh rather than a full rerun.

```markdown
## Candidate Verification Refresh

- Candidate:
- New data checked:
- What changed:
- What did not change:
- Updated gap:
```

```markdown
## Data Gap Note

- Missing field:
- Source attempted:
- Proxy used:
- Impact on conclusion:
```
