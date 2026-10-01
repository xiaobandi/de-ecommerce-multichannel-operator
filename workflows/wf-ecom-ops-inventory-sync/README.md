# 库存同步与超卖预警

> 各平台销量→水位计算→预警推送

工作流 `wf-ecom-ops-inventory-sync`　·　归属 多平台铺货与库存同步　·　触发 定时（每 15 分钟）　·　
复杂度 L　·　阶段 P1

## 调用链

- `库存水位计算`（inventory-waterline-calc）
- `超卖预警`（oversell-alert）

## 文档

- [`WORKFLOW.md`](WORKFLOW.md) — 完整编排、闸门、降级与验收标准
- [`tests/cases.yaml`](tests/cases.yaml) — 端到端 / 闸门 / 降级用例

## 一句话试用

```text
帮我跑 多平台铺货与库存同步 的「库存同步与超卖预警」：<你的输入>
```
