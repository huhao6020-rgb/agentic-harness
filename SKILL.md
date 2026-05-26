---
name: agentic-harness
description: Use when running Codex or Claude Code agentic development workflows for existing project iteration, bug fixes, module development, greenfield bootstrap, platform selection, AGENTS.md, CLAUDE.md, docs/agentic-dev, memory, prd-test-writer, Graphify, OpenSpec, Spec-Kit, Superpowers, UI/UX validation, Task Queue Harness, TASKS.md, task-runner loops, optional /goal compatibility, long-running tasks, or multi-agent coordination. 用于智能体项目 harness。
---

# Agentic Harness

## 核心原则

先把目的聊透，再让 Agent 长时间执行。Harness 的价值不是多写文档，而是让 Codex / Claude Code 每次进入项目时知道：用户真正想要什么、当前做到哪里、该读哪些状态、该用哪个工具、哪些事不能做、怎样才算完成。

本 skill 是调度层，不替代 `prd-test-writer`、Graphify、OpenSpec、Spec-Kit、Superpowers 或 `ui-ux-pro-max`。它负责决定何时启用它们，以及把产物沉淀到哪里。`agent-reach` 只作为当前环境的外部搜索能力，不写入项目级依赖。

## 强制顺序

```text
初始化阶段门禁
-> 规则恢复
-> 深聊对齐
-> 三场景路由
-> 复杂度分级
-> 执行
-> 验证写回
```

1. 初始化阶段门禁：初始化、bootstrap、新项目必须先读 `references/bootstrap-phases.md`，按 7 阶段输出“初始化阶段报告”。
2. 规则恢复：读取已有 `AGENTS.md` / `CLAUDE.md`、`docs/agentic-dev/INDEX.md`、`ACTIVE.md`、`memory/`。
3. 深聊对齐：tiny/small 只确认目标、范围、验收点；medium/large/greenfield 必须先用 `prd-test-writer` 或等价流程完成 PRD、用户故事、验收标准和测试基准。
4. 三场景路由：判断是原有项目迭代、当前项目单模块开发，还是全新项目开发。
5. 复杂度分级：按 `tiny` / `small` / `medium` / `large` / `greenfield` 控制文档、规范、任务队列和多代理的重量。
6. 执行：按场景路由到 debugging、planning、UI/UX、TDD、Task Queue Harness 或 scoped subagents。
7. 验证写回：有测试、构建、lint、浏览器验收、OpenSpec verify 或等价证据后，更新 `ACTIVE.md`、`prd-gate.md`、spec 状态和 `memory/`。

## Reference 路由

- 需要总览 harness 闭环和三场景主链路：读 `references/workflow.md`。
- 需求模糊、中大型任务、长跑前、PRD/验收不清：读 `references/deep-alignment.md`。
- 用户中途补充信息、改变方向、要求记录共识或同步规则：读 `references/purpose-and-iteration.md` 和 `references/rules-and-memory.md`。
- 初始化、bootstrap、新项目、第一次接入 harness：优先读 `references/bootstrap-phases.md`。
- 第一次接入项目、新建项目、初始化框架、缺少 Graphify / Spec 状态：读 `references/bootstrap-contract.md`。
- 判断三类开发入口：读 `references/scenario-router.md`。
- 安装或初始化 skills / CLI / 框架：读 `references/install-skills.md`。
- 创建、审查或同步 `AGENTS.md`、`CLAUDE.md`、`memory/`：读 `references/rules-and-memory.md`。
- 选择 OpenSpec 或 Spec-Kit：读 `references/spec-layer.md`。
- 需要 Codex / Claude Code 平台细节：读 `references/codex-adapter.md` 或 `references/claude-code-adapter.md`。
- 不确定该用哪个 skill：读 `references/skill-routing.md`。
- 长跑、测试迭代、继续开发、无人值守、任务队列：优先读 `references/task-runner-loop.md`。
- 用户显式要求官方 `/goal` 兼容：读 `references/task-runner-loop.md` 的兼容说明，且仍以 `TASKS.md` 为事实源。
- 多代理协作：读 `references/multi-agent-policy.md`。
- 担心流程过重：读 `references/complexity-budget.md`。
- 完成前或修订 skill 后自检：读 `references/validation-playbook.md`。

## 硬门禁

- 未完成平台选择，不初始化平台目录。
- 初始化过程必须输出“初始化阶段报告”；不得只用一句话宣布完成。
- Codex 项目级 skill 成功标准是 `.agents/skills/<skill-name>/SKILL.md` 存在；只有 `.agents/skills/README.md` 不算成功。
- 不在项目内创建 `.codex/skills`；Codex 项目级 skills 使用官方 `.agents/skills`。
- medium / large / greenfield 未完成深聊对齐，不允许 Task Queue 长跑、多代理、Spec-Kit implement 或 OpenSpec apply。
- 同一任务不能同时使用 OpenSpec 和 Spec-Kit。
- 小 bug、小文案、小配置修改不要创建 PRD、Spec、Task Queue、`/goal`、多代理或完整 run ledger。
- 只要 `TASKS.md` 或任务文件中存在 `ready` / `verify_failed` task，不得声称整体完成。
- 多代理必须先有 `TASKS.md`、互不重叠的 write scope 和独立 verifier。
- `.codex/` 只用于官方 Codex config/hooks，不存放自定义 goal、memory 或 tasks。
- 声称完成前必须给出验证证据；未验证只能说“已实现待验证”或“阻塞于某项验证”。
