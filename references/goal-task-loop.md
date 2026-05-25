# Goal / Task 长任务循环

## 核心原则

长任务不是“让 Agent 一直写代码”，而是把高密度前置思考换成可恢复、可验证、可续跑的执行时间。Goal 追求的是持续接近用户结果，不是机械完成任务清单。

## 启用条件

只有满足以下条件才允许官方 `/goal` 或长时间无人值守执行：

- 任务是 large / greenfield / 多模块 medium。
- `prd-gate.md` 已 aligned。
- Spec-Kit 或 OpenSpec 的当前任务边界明确。
- 有 tasks、verification matrix、允许写入范围、停止条件。
- 用户允许长跑。

## 禁止

- 小 bug、小文案、小配置。
- 用户目标不清。
- 没有 PRD / 验收标准。
- 没有验证命令。
- 没有明确 handoff 写回位置。

## 状态文件

长任务至少维护：

```text
docs/agentic-dev/ACTIVE.md
docs/agentic-dev/prd-gate.md
docs/agentic-dev/spec-system.md
docs/agentic-dev/tasks/
docs/agentic-dev/verification/
```

`runs/` 只在需要记录多轮执行日志时创建，不是小任务默认目录。

## `ACTIVE.md` 必写字段

```md
# 当前 Harness 状态

## 当前目标

## 用户结果

## 上游事实源
- PRD：
- Spec：
- Graphify：

## 当前阶段

## 允许写入范围

## 禁止范围

## 下一步

## 验证标准

## 停止条件

## 阻塞项
```

## 每轮结束

- 写清完成了什么、未完成什么。
- 写清验证结果和失败项。
- 写清下一轮入口。
- 不因时间耗尽而声称完成。
- 如果用户中途改变方向，更新 PRD / Spec / tasks，而不是继续旧方向。
