# 多代理协作策略

## 启用条件

只有同时满足以下条件才启用多代理：

- 任务是 large / greenfield，或 medium 任务有明显互不重叠的子任务。
- 已完成深聊对齐，`prd-gate.md` 状态为 `aligned`。
- 已有 `TASKS.md`、单任务 write scope、验证标准。
- 子任务之间没有共享写入文件。
- 有独立 verifier。

## 禁止启用

- 小 bug、小文案、小配置。
- 需求不清、验收不清。
- 多个 agent 需要改同一个核心文件。
- 没有明确 `TASKS.md` 或 write scope。
- 只是想“加速”但无法独立验证。

## 角色

| 角色 | 责任 | 写权限 |
| --- | --- | --- |
| Coordinator | 维护 `TASKS.md`、分配 scope、汇总结果 | `docs/agentic-dev/` |
| Architecture | 读 Graphify、检查边界 | 默认只读 |
| PRD/Test | 使用 `prd-test-writer` 或核对验收 | `docs/prd/`、`prd-gate.md` |
| Implementer | 从 `TASKS.md` 领取单个 scoped task 并实现 | 仅声明范围 |
| UI | 前端体验与浏览器验收 | 仅 UI 范围 |
| Debugger | 复现和定位问题 | 仅修复范围 |
| Verifier | 独立验证 | 默认只读 |
| Documenter | 写回 memory 和 handoff | `docs/agentic-dev/`、`memory/` |

## 执行格式

多代理开始前必须写清：

```md
# 多代理任务

## 共同目标

## 上游事实源
- PRD：
- Spec：
- Graphify：

## 任务队列
- Source of truth：`docs/agentic-dev/TASKS.md`
- 当前只允许领取状态为 `ready` 或 `verify_failed` 的任务。

## 任务分配
| Agent | Task ID | 任务 | Write Scope | 禁止范围 | 验证方式 |
| --- | --- | --- | --- | --- | --- |

## 汇总规则
- Implementer 不自行宣布完成。
- Verifier 独立验证后才能合并结论。
- 只有验证通过的 task 才能标记 `done`。
- 验证失败的 task 必须标记 `verify_failed` 并记录下一轮入口。
- 冲突或 scope 重叠时停止并重新分配。
```

## 写回

结束后更新：

- `ACTIVE.md`
- `TASKS.md`
- `prd-gate.md`
- `spec-system.md`
- `docs/agentic-dev/verification/`
- `docs/agentic-dev/handoff.md`
- `memory/modules/`
