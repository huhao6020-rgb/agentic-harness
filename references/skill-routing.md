# Skill 路由

## 先判断目的

不要看到关键词就直接开工。先判断用户要的是性能、交互、视觉、架构、bug、模块能力、全新产品，还是项目框架初始化。

## 路由表

| 场景 / 信号 | 使用能力 | 前置条件 |
| --- | --- | --- |
| 需求模糊、PRD、验收标准、测试基准 | `prd-test-writer` | medium 以上必须 |
| 结构不清、模块边界不清、影响范围不明 | Graphify | 有代码/文档可分析；否则记录待生成 |
| UI/UX、交互、视觉、响应式、前端体验 | `ui-ux-pro-max` | 先有用户目标和验收信号 |
| bug、异常、测试失败、性能问题 | systematic-debugging | 先复现或明确现象 |
| 新功能或 bugfix 实现 | test-driven-development | 用户未要求跳过时默认使用 |
| 多步骤实现计划 | writing-plans | 需求和验收已明确 |
| 多代理协作 | subagent-driven-development | 有 `TASKS.md`、write scope、独立 verifier |
| 长跑、继续开发、测试失败回流 | Task Queue Harness | 读 `task-runner-loop.md`，以 `TASKS.md` 为事实源 |
| 完成前 | verification-before-completion | 必须有验证命令或等价证据 |
| 外部搜索 | web / 当前环境可用搜索能力 | 不写入项目依赖 |

## 规范系统路由

| 场景 | 默认 |
| --- | --- |
| 小 bug / 小文案 / 小配置 | 不用 OpenSpec / Spec-Kit |
| 原有项目 medium+ 迭代 | `prd-test-writer` -> OpenSpec |
| 当前项目单模块 medium+ 开发 | Graphify -> `prd-test-writer` -> OpenSpec |
| 全新项目 | `prd-test-writer` -> Spec-Kit |

## 示例

- “优化自由画布”：先问优化目标，不直接路由。
- “自由画布拖拽卡顿”：systematic-debugging，必要时 Graphify。
- “自由画布交互不好用”：`prd-test-writer` 轻量澄清 + `ui-ux-pro-max`。
- “重构画布架构”：Graphify + `prd-test-writer` + OpenSpec。
- “构建自由画布模块”：Graphify + `prd-test-writer` + OpenSpec。
- “新建画布应用”：平台选择 + bootstrap + `prd-test-writer` + Spec-Kit。

## 禁止

- 不把 `agent-reach` 当项目级依赖。
- 不为小任务启用多代理、Task Queue、Spec-Kit 或 OpenSpec。
- 不在 PRD 门禁未通过时启动长跑执行。
