---
name: fangtang-a-share-leader-radar
description: 当用户需要分析 A 股市场主线、强势板块、题材候选、股票池或具体个股时使用本 skill。支持单股分析、多股比较、股票池筛选、全市场筛选和板块复盘；数据路由遵循 TdxQuant 优先、通达信 RPS 优先、Wind 增强、同花顺和东财妙想配额受控使用的原则。
---

# 方塘 A 股主线雷达（Fangtang A-Share Leader Radar）

本 skill 以无数据源版框架为分析基线。框架保持稳定，数据源根据任务规模、数据类型、平台可用性和调用预算来选择。

## 使用流程

1. 先阅读 `references/Fangtang A-Share Leader Radar（无数据源版）v0.8.md`，明确分析框架。
2. 判断任务类型：单股分析、多股比较、股票池筛选、全市场筛选或板块复盘。
3. 查询前阅读 `references/data-routing.md`，输出前阅读 `references/data-contract.md`。相对强度优先使用 `tdx-rps-query`；凡是 `TdxQuant` 能提供的数据，优先使用 `TdxQuant`，除非用户明确指定其他来源。
4. 涉及 `TdxQuant` 和 RPS 时，阅读 `references/tdxquant-and-rps-adapter.md`。`TdxQuant` 的实时快照和行情订阅只作为可选的有界模式，不作为默认批量筛选路径。
5. 使用同花顺或东财妙想 skills 前，阅读 `references/quota-aware-data-sources.md`。需要判断增强调用数量时，阅读 `references/source-budget.md`。
6. 在 `TdxClaw` 或 `WindClaw` 环境执行时，先阅读 `references/platform-adapters.md`，再阅读对应平台执行文档。
7. 按框架顺序执行：市场环境、主线板块、个股初筛或角色初判、个股详细分析、横向比较、信息缺口。
8. 需要具体输出结构时，阅读 `references/task-templates.md`。
9. 需要判断先查什么、何时缩小候选、何时停止扩查时，阅读 `references/workflows.md`。
10. 需要参考请求如何映射到流程、数据路线、预算模式和输出模板时，阅读 `references/examples.md`。
11. 输出必须绑定数据范围，并明确说明数据缺口、降级数据源和混合数据日期。

## 数据源优先级

- 相对强度：个股和通达信行业板块 RPS 优先使用 `tdx-rps-query`。
- 行情、K 线、快照、股票列表、板块、公式和可取得的财务/F10 数据：优先使用 `TdxQuant`。
- Wind：用于规范化财务比较、专业资料、研究语境、一致预期或 `WindClaw` 执行增强。
- 同花顺和东财妙想：作为配额受控的增强数据源，用于题材语言、自然语言筛选、公告、新闻、研报、热点、行业背景和单股研究；不能作为全市场批量筛选底座。
- 公共 A 股数据源：仅在主路由不可用、需要公开披露事实、或某公共源明显更适合对应字段时使用。

## 资源文件

- `references/Fangtang A-Share Leader Radar（无数据源版）v0.8.md`：平台无关的分析框架。
- `references/data-routing.md`：按任务和分析模块定义数据源优先级。
- `references/data-contract.md`：跨平台数据口径和输出口径。
- `references/tdxquant-and-rps-adapter.md`：`TdxQuant` 优先和 RPS 优先的执行说明。
- `references/quota-aware-data-sources.md`：同花顺和东财妙想的配额使用策略。
- `references/source-budget.md`：按任务规模和数据源类型定义调用预算。
- `references/platform-adapters.md`：`TdxClaw` 和 `WindClaw` 的适配说明。
- `references/tdxclaw-execution.md`：通达信/`TdxClaw` 工具映射、调用批次和失败处理。
- `references/windclaw-execution.md`：`WindClaw` 工具映射和 Wind 增强执行路径。
- `references/task-templates.md`：各任务模式的输出骨架。
- `references/workflows.md`：任务执行流程卡和停止扩查规则。
- `references/examples.md`：请求到流程的映射示例，不包含虚构行情结论。
- `references/shared-a-share-data-sources.md`：公共 A 股数据源兜底表。
