# 复杂度预算

## 原则

流程重量必须匹配任务重量。Harness 的目的不是制造文档，而是让任务可恢复、可验证、可继续。

## 分级

| 级别 | 典型任务 | 允许产物 | 禁止 |
| --- | --- | --- | --- |
| tiny | 文案、样式微调、单行配置 | 最终说明和验证 | PRD、Spec、goal、多代理 |
| small | 小 bug、小迭代 | 简短计划或验证记录 | Spec-Kit、OpenSpec、run ledger |
| medium | 明确模块/行为变更 | `prd-gate.md`、PRD、OpenSpec、plan/tasks | 无 PRD 就执行 |
| large | 多文件、多模块、长任务 | PRD、Spec、verification matrix、`/goal`、scoped subagents | 无 verifier 的多代理 |
| greenfield | 全新项目 | bootstrap、PRD、Spec-Kit、memory | 未确认平台就初始化 |

## 场景绑定

- 场景一默认 tiny / small；只有行为边界变化才升级 medium。
- 场景二默认 medium；如果只是模块内小修，降级 small。
- 场景三默认 greenfield；即使 MVP 很小，也必须有平台选择、bootstrap 和 PRD 门禁。

## 升级条件

满足任一条件就升级：

- 用户目的或验收标准需要深聊。
- 影响接口、数据、权限、状态流或多个模块。
- 需要 UI/UX 判断和浏览器验收。
- 需要无人值守长跑、`/goal` 或多代理。

## 降级条件

满足以下条件应降级：

- 只改一处小 bug 或文案。
- 不改变行为边界。
- 不需要新增目录、规范系统或任务队列。
- 能用一个验证命令确认完成。

## 红线

- 不能为了显得完整给小任务生成大量文档。
- 不能为了省事让大任务跳过 PRD 门禁。
- 不能把“跑了 30 分钟”当作完成证据。
