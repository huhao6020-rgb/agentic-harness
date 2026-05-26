# Codex 适配

## 平台入口

Codex 项目使用 `AGENTS.md`。不要在用户选择 Codex-only 时强制创建 `.claude/` 或 Claude Code 专属文件。

推荐分层：

- 全局：`~/.codex/AGENTS.md`
- 项目：项目根目录 `AGENTS.md`
- 子目录：模块级 `AGENTS.md`

项目级 skills 使用：

```text
.agents/skills/
```

`.codex/` 只用于官方 Codex config/hooks，不存放自定义 goal、memory、tasks 或 harness 状态。

## `AGENTS.md` 应该写什么

- 平台和项目级 skills 路径。
- 启动顺序：`INDEX.md`、`ACTIVE.md`、`prd-gate.md`、`spec-system.md`、`graphify.md`；长跑任务再读 `TASKS.md`、`task-runner.md`、`handoff.md`。
- 深聊对齐门禁。
- OpenSpec / Spec-Kit 二选一。
- Graphify、UI/UX、debugging、多代理、验证路由。
- 完成标准。

## Codex 启动检查

1. 当前平台是否为 Codex 或双平台。
2. `.agents/skills/` 是否包含所需项目级 skills。
3. `prd-gate.md` 是否 aligned；medium 以上未 aligned 不进入实现。
4. `spec-system.md` 是否选择唯一规范系统。
5. `ACTIVE.md` 是否有允许写入范围和验证标准。
6. 长跑任务是否存在 `TASKS.md`，并且没有未处理的 `ready` / `verify_failed` task。

## 长任务

Codex 默认使用 `docs/agentic-dev/TASKS.md` 驱动长跑。官方 `/goal` 只在用户显式要求时作为兼容层，并且必须已有：

- PRD 或等价深聊记录。
- `TASKS.md`。
- `task-runner.md`。
- `verification-matrix.md`。
- write scope。
- stop conditions。

执行时优先处理 `verify_failed`，再处理最高优先级 `ready`。只要队列中存在 `ready` 或 `verify_failed`，不得声称整体完成。

不要创建自定义 `.codex/goals` 或 `.codex/tasks`。
