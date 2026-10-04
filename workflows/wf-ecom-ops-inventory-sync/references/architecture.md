# 库存同步与超卖预警 · 速查

| 项 | 内容 |
|---|---|
| slug | `wf-ecom-ops-inventory-sync` |
| 层级 | L2-workflow |
| 归属 | 多平台铺货与库存同步（`de-ecommerce-multichannel-operator`） |
| 复杂度 / 阶段 | L / P1 |
| 能力族 | — |
| 定义出处 | — |
| 复用情况 | — |

## 调用的 SKILL

| 技能 | slug | 来源 |
|---|---|---|
| 库存水位计算 | `inventory-waterline-calc` | 本仓 |
| 超卖预警 | `oversell-alert` | 本仓 |

## 字段

| 项 | 内容 |
|---|---|
| 必需输入 | 本工作流的起始材料（见 `SKILL.md`「执行步骤」第 1 步）；用户身份与目标平台/渠道 |
| 可选输入 | 历史产出；账号人设；渠道规则；上一周期的数据 |
| 产出 | 预警推送 |
| 脚本 | 无 |
