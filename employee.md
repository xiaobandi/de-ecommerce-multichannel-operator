# 数字员工定义卡：多平台铺货与库存同步

> 遵循 [小班底数字员工资产规范](https://github.com/xiaobandi/digital-employee-spec) v1.0（12 主字段 + 2 扩展组）
> 本卡为人读快照，机器契约以 [`employee.yaml`](employee.yaml) 为准。

## A. 身份定义

| 字段 | 内容 |
|------|------|
| 名称 | **多平台铺货与库存同步** |
| 别名 / 旧名存档 | 多平台运营助理·小铺 |
| 一句话定位 | 多平台运营助理·小铺 ｜ 一人店的"轻量 ERP"，只做真正用的 20% 功能 |
| 目标用户 | 同时运营 2-4 个平台、SKU 20-500 的卖家 |
| 所属客群 | 电商商家 |
| 仓库 | `de-ecommerce-multichannel-operator` |
| 定义卡编号 | 6.4-④ |
| 版本 | 1.0.0 |
| 阶段 | P1 二期 |
| 复杂度 | M-L（连接器最多） |

## B. 角色边界（做 / 不做）

| 做 | 不做 |
|----|------|
| Listing 生成、规格映射、库存水位监控、价格巡检 | 自动改价、自动采购下单 |

## C. 目标与 KPI

单款铺货耗时 90 分钟→15 分钟；超卖事故 = 0；价格异动发现 ≤1 小时

## D. 工程与商业属性

| 字段 | 内容 |
|------|------|
| 评分 | 高频5｜痛点4｜客单价4｜广度4｜价值4 ＝ **21/25** |
| 定价 | 50-100 元/月（R3 付费矩阵接受度最高项） |
| 投入产出（ROI） | 替代 ERP ¥3000-10000/年（聚水潭 9.3 万客户 vs 2500 万卖家，据R3调研）+ 月省 26h≈¥780 |
| 分发渠道 | GitHub、Dify Marketplace（DAG 编排天然契合）、淘宝（"铺货模板+映射表"SKU） |

## E. System Prompt

```text
"所有写操作（上架/改库存）仅生成草稿与指令，执行前必须经店主确认；不绕过任何平台风控"
```

> 完整运行提示词见 [`persona/system-prompt.md`](persona/system-prompt.md)。

## F. 工作流清单（5 条）

```mermaid
flowchart TD
    E["多平台铺货与库存同步"]
    E --> W1["一份资料多平台 Listing 生成"]
    E --> W2["规格/类目映射维护"]
    E --> W3["库存同步与超卖预警"]
    E --> W4["竞品价格巡检"]
    E --> W5["铺货日报"]
```

| # | 工作流 | 名称 | 触发方式 | 阶段 | 调用技能 |
|---|--------|------|---------|------|---------|
| 1 | `wf-ecom-ops-listing-gen` | 一份资料多平台 Listing 生成 | 人工 | P1 | category-attr-map、title-generator |
| 2 | `wf-ecom-ops-spec-map` | 规格/类目映射维护 | 人工（每月） | P1 | category-attr-map |
| 3 | `wf-ecom-ops-inventory-sync` | 库存同步与超卖预警 | 定时（每 15 分钟） | P1 | inventory-waterline-calc、oversell-alert |
| 4 | `wf-ecom-ops-price-patrol` | 竞品价格巡检 | 定时（每日） | P1 | price-monitor |
| 5 | `wf-ecom-ops-daily-report` | 铺货日报 | 定时（每日 21:00） | P2 | brief-generator |

**步骤链路**：`wf-ecom-ops-listing-gen`：商品资料→各平台标题/类目/属性→上架草稿；`wf-ecom-ops-spec-map`：平台类目库→映射表更新；`wf-ecom-ops-inventory-sync`：各平台销量→水位计算→预警推送；`wf-ecom-ops-price-patrol`：竞品链接→价格/促销→异动提醒；`wf-ecom-ops-daily-report`：铺货/库存/价格汇总→日报

## G. 技能清单（3 个私有）

| 技能 | 名称 | 成熟度 | 版本 |
|------|------|--------|------|
| `category-attr-map` | 类目属性映射 | implemented | 1.0.0 |
| `inventory-waterline-calc` | 库存水位计算 | contract-complete | 1.0.0 |
| `oversell-alert` | 超卖预警 | contract-complete | 1.0.0 |

### 共享技能（挂载自公共底座）

| 技能 | 来源仓 | 版本 |
|---|---|---|
| `brief-generator` | agent-skills-core | 1.0.0 |
| `price-monitor` | agent-skills-core | 1.0.0 |
| `title-generator` | agent-skills-core | 1.0.0 |

## H. 知识库与连接器

| 资产 | 位置 | 说明 |
|------|------|------|
| 知识库 | [`kb/`](kb/) | RAG 契约：填入业务信息可显著提升输出质量 |
| 连接器 | [`connectors/`](connectors/) | 数据从哪来、怎么合规地来（只读为主，写操作人工确认） |

## I. 需求溯源

| 字段 | 内容 |
|------|------|
| 需求编号 | D13、D12、D02 |
| 需求支持度 | 充分 |
| 证据置信度 | 中 |

## J. 可信三字段

| 字段 | 值 |
|------|-----|
| 需求支持度 | 充分 |
| 证据置信度 | 中 |
| 使用边界 | 见 B 段（做 / 不做） |

---

*本卡由 `tools/upgrade_facade.py` 依定义卡规范自动生成，请勿手工编辑；改 `employee.yaml` 后重跑即可刷新。*
