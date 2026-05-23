# TdxClaw 执行参考

当方塘雷达运行在 `TdxClaw` 或通达信工具链较完整的环境中，使用本文件。分析框架仍来自无数据源版，数据优先级仍来自 `data-routing.md`。

## 执行顺序

1.  前运行环境能调用 `TdxQuant` 时，优先使用 `TdxQuant` 覆盖的数据。
2. 个股 RPS 和通达信行业板块 RPS 使用 `tdx-rps-query`。
3.   `TdxClaw` 暴露的是 `tdx_*` 工具时，用这些工具承接通达信侧行情、板块、F10、公式和筛选工作。
4. 故事、题材、研报、公告和研究缺口，使用配额受控的同花顺或东财妙想 skills。
5. 只有出现实质字段缺口时，才用公共源兜底。

## 工具映射

| 需求 | `TdxClaw` 工具 | 说明 |
|---|---|---|
| 股票和板块查找 | `tdx_lookup_stock` | K 线调用前先解析模糊名称和指数代码 |
| 市场生态和自然语言筛选 | `tdx_screener` | 适合涨停、跌停、连板、板块关键词和聚焦筛选 |
| 报价和交易状态 | `tdx_quotes` | 使用成交额、换手、市值、价格、涨跌幅等字段 |
| K 线和趋势结构 | `tdx_kline` | 多股比较时统一日线和周线窗口 |
| F10、板块、行业、题材、资金、股东 | `tdx_api_data` | 优先使用具体 entry 和 tag 做结构化检查 |
| 财务字段快速选择 | `tdx_indicator_select` | 效率高，但要防止重试风暴 |
| 相对强度 | `tdx-rps-query` | 直接 RPS 路线；不要把涨跌幅代理称为 RPS |

## 框架映射

| 框架步骤 | 优先 `TdxClaw` 路线 |
|---|---|
| 市场环境 | 指数 K 线、市场生态筛选、必要时行业 RPS |
| 主线板块 | 板块筛选、板块/行业数据、板块 K 线、行业 RPS |
| 个股初筛 | RPS 批量查询、报价、K 线、聚焦筛选结果 |
| 基本面 | `TdxQuant` 可得财务优先，其次 `tdx_indicator_select` 或结构化 `tdx_api_data` |
| 故事面 | 通达信题材和事件字段；必要时用配额型题材/新闻/研报来源补缺 |
| 动量面 | 个股 RPS + 日线/周线 K 线结构 |
| 资金面 | 报价和价量承接优先；结构化资金、股东、龙虎榜、两融字段作为辅助 |

## 有界调用批次

### 市场扫描

候选扩展前，先做一次市场扫描：

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

调用 `tdx_kline` 前先解析指数代码。旧版 `TdxClaw` 草稿记录过：中证 1000 等代码可能在跳过 lookup 时被误读。

### 板块扫描

市场扫描识别出方向后，再做板块检查：

```text
tdx_screener message="<行业或题材关键词>"
tdx_kline code="<板块指数>" setcode="<market>" period="4" wantNum="60"
tdx-rps-query --scope industry --industry-names "<通达信行业名>" --metrics HY_RPS20,HY_RPS60 --json
```

 运行环境暴露结构化板块和行业 `tdx_api_data` 时，可用于板块详情、产业链、阶段涨幅和重要行业事件。

### 候选扫描

昂贵的候选增强前，先批量查询 RPS：

```text
tdx-rps-query --scope stock --codes <codes> --metrics RPS5,RPS20,RPS60,RPS120,RPS250 --json
tdx_quotes code="<code>" setcode="<market>"
tdx_kline code="<code>" setcode="<market>" period="4" wantNum="250"
tdx_kline code="<code>" setcode="<market>" period="5" wantNum="60"
```

只有入围短名单才做题材、研报、股东、资金流等更宽的增强检查。

## 常用结构化检查

旧版 `TdxClaw` 草稿中使用过以下 `tdx_api_data` 模式。仅在 前运行环境仍暴露该 entry 且字段能解决实质缺口时使用。

| 检查 | 示例模式 |
|---|---|
| 题材标签 | `entry="TdxSharePCCW.tdxf10_gg_rdtc"`，如 `zttzztk` |
| 事件标签 | `entry="TdxSharePCCW.tdxf10_gg_rdtc"`，如 `sjcd` |
| 利润表 | `entry="TdxShareCW.ph_agf10_cw_lyb"`，报告期或单季 tag |
| 资产负债表 | `entry="TdxShareCW.ph_agf10_cw_zcfzb"` |
| 现金流量表 | `entry="TdxShareCW.ph_agf10_cw_xjllb"` |
| 主营构成 | `entry="TdxShareCW.ph_agf10_jyfx"` |
| 业绩预警 | `entry="TdxSharePCCW.tdxf9_ag_cwsj_yjyj"` |
| 估值历史 | `entry="TdxShareCW.ph_agf10_gzfx"` |
| 行业估值排名 | `entry="TdxShareCW.ph_agf10_hypm"` |
| 资金流、北向、两融、龙虎榜 | 如 `tdxf10_gg_jyds` 交易数据族 |
| 股东人数和十大股东 | 如 `tdxf10_gg_gdyj` 股东数据族 |

这些只是适配提示，不承诺每个 `TdxClaw` 环境都保持同名 entry。

## 失败处理

- `tdx_indicator_select` 超时或报服务错误时，最多重试三次，间隔递增，并控制并发。之后降级到结构化 `TdxQuant`/`TdxClaw` 字段或其他聚焦来源。
- `tdx_screener` 返回的是生态代理变量而不是真实全市场宽度时，必须标注为代理。
- RPS 槽位过期或不可用时，只能使用明确标注的涨跌幅代理。
- 一个板块名称映射到多个概念或行业时，说明采用了哪条通达信板块或题材路线。
