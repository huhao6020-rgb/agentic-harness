# Task Runner Loop 长跑协议

## 核心原则

长时间开发默认使用 Task Queue Harness，而不是 `/goal`。`/goal` 可以作为平台能力被显式调用，但不能替代项目内的任务队列事实源。

Task Queue Harness 的目的不是让 Agent 一直写代码，而是让每一轮工作都可恢复、可验证、可继续：

```text
PRD / 测试基准
-> Spec / 架构约束
-> TASKS.md
-> 单任务执行
-> 验证失败回流
-> 继续下一个 task
-> handoff 可恢复
```

## 启用条件

满足任一条件时启用：

- 用户要求长跑、继续开发、无人值守、自动迭代或测试驱动迭代。
- 任务是 large / greenfield / 多模块 medium。
- 存在多轮实现、验证、修复和写回需求。
- 需要 Codex 与 Claude Code 共享同一套恢复状态。

小 bug、小文案、小配置不要创建完整 Task Queue；只做最小计划和验证。

## 状态文件

medium / large / greenfield 进入长跑时，默认维护：

```text
docs/agentic-dev/ACTIVE.md
docs/agentic-dev/TASKS.md
docs/agentic-dev/task-runner.md
docs/agentic-dev/handoff.md
docs/agentic-dev/verification-matrix.md
docs/agentic-dev/tasks/
```

`TASKS.md` 是唯一任务队列事实源。`task-runner.md` 是执行协议。`handoff.md` 写停止原因和下一轮入口。`verification-matrix.md` 写每个 task 的验证状态。

## Task 状态

只允许以下状态：

| 状态 | 含义 | 下一步 |
| --- | --- | --- |
| `ready` | 可以执行 | 按优先级选择 |
| `in_progress` | 当前正在执行 | 继续或恢复 |
| `verify_failed` | 已实现但验证失败 | 下一轮优先修复 |
| `blocked` | 被外部条件阻塞 | 写清 blocker 和解除条件 |
| `done` | 验证通过 | 进入下一个 ready task |
| `skipped` | 用户明确跳过 | 写明跳过原因 |

只要存在 `ready` 或 `verify_failed` task，就不得声称整体完成。`smoke-pass` 只能作为某个 task 的验证证据，不能代表项目完成。

## `TASKS.md` 模板

```md
# 任务队列

## 队列规则
- 优先处理 `verify_failed`，再处理最高优先级 `ready`。
- 每次只执行一个当前 task，除非明确启用 scoped subagents。
- 不允许跳过验证直接标记 `done`。
- `blocked` 必须写清解除条件。

## 当前队列
| ID | 状态 | 优先级 | 标题 | Write Scope | 验证方式 | 依赖 |
| --- | --- | --- | --- | --- | --- | --- |
| T001 | ready | P0 |  |  |  |  |

## 完成门禁
- 没有 `ready` 或 `verify_failed` task。
- 所有非 skipped / blocked task 均为 `done`。
- `verification-matrix.md` 中没有未解释的失败项。
```

## 单任务模板

```md
# T001 任务标题

## 状态
ready

## 用户结果

## 上游事实源
- PRD：
- Spec：
- Graphify：

## 允许写入范围

## 禁止范围

## 验收标准

## 验证命令

## 完成证据

## 失败处理

## 下一个任务
```

## 执行循环

每轮必须按顺序执行：

```text
读取 AGENTS.md / CLAUDE.md
-> 读取 ACTIVE.md、TASKS.md、handoff.md、verification-matrix.md
-> 优先选择 verify_failed task，否则选择最高优先级 ready task
-> 检查 PRD、Spec、Graphify、write scope、禁止范围
-> 将 task 标记为 in_progress
-> 实现当前 task
-> 运行验证命令
-> 验证通过：标记 done，记录完成证据
-> 验证失败：标记 verify_failed，记录失败日志和下一轮修复入口
-> 外部阻塞：标记 blocked，写清解除条件
-> 更新 ACTIVE.md、TASKS.md、verification-matrix.md、handoff.md
-> 如果还有 ready / verify_failed task，继续下一项
```

## 停止条件

只有以下原因允许停止：

- 没有 `ready` 或 `verify_failed` task。
- 当前最高优先级 task 被 `blocked`，且没有其他可执行 task。
- 用户要求暂停或改变方向。
- 权限、依赖、网络、密钥或验证环境不可用。
- 达到本轮运行预算，但必须写清下一轮入口。

停止时必须在 `handoff.md` 写明：

```md
# Handoff

## 为什么停止

## 当前 task

## 已完成

## 未完成

## 验证结果

## 阻塞项

## 下一轮入口
```

## 测试迭代规则

- 测试、构建、lint、typecheck、浏览器验收失败时，不得标记 `done`。
- 失败必须回流为 `verify_failed` task 或拆分新的 `ready` 修复 task。
- 修复验证失败时，优先补充可复现测试或最小验证脚本。
- 验证命令不存在时，先创建或记录等价验证方式，不得声称已验证。

## 多代理关系

多代理只从 `TASKS.md` 领取任务。Coordinator 必须先为每个 Implementer 分配互不重叠的 write scope；Verifier 独立验证后，Documenter 才能写回 `done`。

## Codex / Claude Code 适配

- Codex：直接读取 `AGENTS.md` 和 `docs/agentic-dev/TASKS.md`，项目级 skills 放 `.agents/skills/`。
- Claude Code：读取 `CLAUDE.md`；如使用 `/loop`，让 `.claude/loop.md` 执行同一个 task runner prompt。
- 两个平台都不得把长跑状态只放在对话记忆里。

## `/goal` 兼容说明

`/goal` 只在用户显式要求或平台运行方式必须借助官方 goal 能力时使用。即使用 `/goal`，它也只能作为执行引擎，不能成为事实源。

启用前必须已有：

- `docs/agentic-dev/TASKS.md`
- `docs/agentic-dev/task-runner.md`
- `docs/agentic-dev/verification-matrix.md`
- 允许写入范围、停止条件和 handoff 写回位置

禁止：

- 小 bug、小文案、小配置使用 `/goal`。
- 没有 PRD / 验收标准时使用 `/goal`。
- 没有 `TASKS.md` 时使用 `/goal`。
- 用 `smoke-pass` 代表整体完成。
- 创建自定义 `.codex/goals` 或 `.codex/tasks`。

如果使用 `/goal`，第一步仍然是读取本文件，再按 Task Queue Harness 选择下一个 `ready` 或 `verify_failed` task。只要队列中仍有可执行任务，就不得因某个里程碑通过而声称整体完成。
