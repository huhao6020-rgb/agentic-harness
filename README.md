# Agentic Harness

Agentic Harness 是一个面向 Codex / Claude Code 的智能体项目调度 skill。它的核心目标不是替代具体开发工具，而是帮助智能体在进入项目时先恢复规则、对齐目的、选择合适流程，并把执行状态和长期记忆沉淀到正确的位置。

## 适用场景

- 既有项目迭代、bug 修复和单模块开发。
- 新项目 bootstrap、平台选择和项目级入口规则构建。
- PRD、验收标准、Graphify、OpenSpec、Spec-Kit、Superpowers、UI/UX 校验和任务队列长跑的流程路由。
- 多代理协作、长时间无人值守任务、`TASKS.md` / task-runner / handoff 状态恢复。

## 核心流程

每次任务默认按以下顺序推进：

```text
初始化阶段门禁
-> 规则恢复
-> 深聊对齐
-> 三场景路由
-> 复杂度分级
-> 执行
-> 验证写回
```

关键原则：

- 初始化、bootstrap、新项目必须先按 `references/bootstrap-phases.md` 输出 7 阶段初始化报告。
- tiny / small 任务只确认目标、范围和验收点；medium / large / greenfield 必须先完成 PRD、用户故事、验收标准和测试基准。
- 同一任务不能同时使用 OpenSpec 和 Spec-Kit。
- 小 bug、小文案、小配置修改不创建过重流程。
- 声称完成前必须有测试、构建、lint、浏览器验收、OpenSpec verify 或等价证据。

## 目录结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── bootstrap-phases.md
    ├── workflow.md
    ├── scenario-router.md
    ├── complexity-budget.md
    ├── rules-and-memory.md
    └── ...
```

- `SKILL.md`：skill 入口，定义 Agentic Harness 的用途、强制顺序、reference 路由和硬门禁。
- `agents/openai.yaml`：OpenAI / Codex 环境下的展示名称、简介和默认提示词。
- `references/`：按主题拆分的流程说明，包括初始化、三场景路由、复杂度预算、规则记忆、规范系统、长跑任务和验证策略。

## Reference 路由

常用入口：

- 总览 harness 闭环和三场景主链路：`references/workflow.md`
- 初始化、bootstrap、新项目：`references/bootstrap-phases.md`
- 第一次接入项目或构建项目框架：`references/bootstrap-contract.md`
- 判断开发入口：`references/scenario-router.md`
- 复杂度分级与避免流程过重：`references/complexity-budget.md`
- 规则、入口文件和 memory 同步：`references/rules-and-memory.md`
- 长跑、任务队列和恢复入口：`references/task-runner-loop.md`
- 完成前验证：`references/validation-playbook.md`

## 使用方式

在支持 Codex skills 的环境中，将本目录作为 `agentic-harness` skill 使用。触发后，智能体应先读取 `SKILL.md`，再根据当前任务进入对应 reference 文件。

典型调用意图：

```text
使用 agentic-harness 初始化当前项目
使用 agentic-harness 规划这个模块的开发流程
使用 agentic-harness 恢复任务队列并继续执行
使用 agentic-harness 同步项目规则和 memory
```

## 与其他工具的关系

Agentic Harness 是调度层，不替代以下能力：

- `prd-test-writer`：PRD、用户故事、验收标准和测试用例。
- Graphify：代码、文档和架构关系图谱。
- OpenSpec / Spec-Kit：规范系统和变更管理。
- Superpowers：调试、计划、TDD、验证、分支收尾等执行方法。
- `ui-ux-pro-max`：UI/UX 设计和验收智能。

它负责判断何时启用这些工具，以及执行后的状态应该写回 `ACTIVE.md`、`prd-gate.md`、`spec-system.md`、`graphify.md`、`TASKS.md`、`handoff.md` 或 `memory/`。

## 重要约束

- Codex 项目级 skills 使用 `.agents/skills/<skill-name>/SKILL.md`，不在项目内创建 `.codex/skills`。
- `.codex/` 只用于官方 Codex config / hooks，不存放自定义 goal、memory 或 tasks。
- 只要任务队列中存在 `ready` 或 `verify_failed` task，就不能声称项目整体完成。
- 多代理协作必须先有 `TASKS.md`、互不重叠的写入范围和独立 verifier。
- 未完成验证时，只能说明“已实现待验证”或“阻塞于某项验证”。
