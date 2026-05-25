# 三场景路由

## 原则

先判断开发场景，再判断任务重量。三类开发不能混成一套流程。

```text
深聊对齐
-> 场景路由
-> 任务分级
-> 项目理解 / bootstrap
-> 规范 / skill / subagent 选择
-> 验证写回
```

## 场景一：原有项目开发、迭代修改、bug 修复

适用请求：

- 修复当前项目 bug。
- 优化已有功能、性能、交互或测试。
- 在已有模块上做迭代。

默认流程：

```text
读取 AGENTS.md / CLAUDE.md
-> 读取 ACTIVE.md、memory、graphify-out
-> 目的确认：bug 表现、预期行为、验收信号
-> bug / 测试失败 / 性能问题走 systematic-debugging
-> 结构不清时查 Graphify
-> 中型行为变更才进入 prd-test-writer + OpenSpec
-> 最小修改
-> verification-before-completion
-> 写回必要 memory / ACTIVE
```

允许：

- tiny：不创建新文档，只做最小验证。
- small：可写简短 plan / verification。
- medium：可用 `prd-test-writer` + OpenSpec。
- large：允许官方 `/goal`、run ledger、scoped subagents。

禁止：

- 小 bug 不创建 PRD、Spec-Kit、OpenSpec、`/goal` 或多代理。
- 不默认启用 Spec-Kit。
- 需求不清时不直接改代码。

## 场景二：当前项目的单个模块开发

适用请求：

- 当前项目新增明确模块。
- 重做或扩展某个模块。
- 模块有独立入口、接口、页面、状态或数据边界。

默认流程：

```text
读取入口规则和 project-structure.md
-> 明确模块目标、接口、禁止影响范围
-> Graphify 查询模块边界和相邻依赖
-> prd-test-writer 完成 PRD / 用户故事 / 验收 / 测试基准
-> OpenSpec 定义变更
-> plan / tasks / verification
-> scoped subagents 按互不重叠 write scope 执行
-> 独立 verifier 验证
-> 更新 memory/modules/<module>.md
```

允许产物：

```text
docs/prd/
docs/agentic-dev/plans/<module>.md
docs/agentic-dev/tasks/<module>.md
docs/agentic-dev/verification/<module>.md
memory/modules/<module>.md
openspec/
```

禁止：

- 不默认启用 Spec-Kit，除非该“模块”实际是独立新项目。
- 不绕过 `project-structure.md` 创建平行目录。
- 不让实现 agent 修改未声明的相邻模块。
- 未完成 PRD 门禁不进入 OpenSpec apply 或多代理。

## 场景三：全新项目开发

适用请求：

- 从零构建应用、网站、工具、游戏、服务或系统。
- 初始化新代码库。
- 需要决定平台、技术栈、产品目标、目录结构、规范和任务计划。

默认流程：

```text
确认平台：Codex / Claude Code / 双平台
-> 项目级 bootstrap
-> prd-test-writer 深聊产品目标、用户故事、验收标准、测试基准
-> Spec-Kit constitution / specify / clarify / plan / tasks
-> 创建基础代码或文档骨架
-> Graphify 生成或记录待生成状态
-> UI 任务进入 ui-ux-pro-max
-> 执行 / 多代理 / 官方 /goal
-> 验证
-> 更新 ACTIVE、prd-gate、spec-system、memory
```

必须产物：

```text
平台入口：AGENTS.md 或 CLAUDE.md
docs/agentic-dev/INDEX.md
docs/agentic-dev/ACTIVE.md
docs/agentic-dev/project-structure.md
docs/agentic-dev/development-protocol.md
docs/agentic-dev/graphify.md
docs/agentic-dev/spec-system.md
docs/agentic-dev/skill-manifest.md
docs/agentic-dev/prd-gate.md
docs/agentic-dev/decisions.md
memory/project.md
memory/decisions.md
memory/constraints.md
memory/glossary.md
memory/modules/
```

禁止：

- 未确认平台就创建 `.agents/` 或 `.claude/`。
- 跳过 `prd-test-writer` 或等价深聊直接写代码。
- 新项目默认初始化 OpenSpec。
- 同时初始化 OpenSpec 和 Spec-Kit。
- 创建自定义 `.codex` goal、memory 或 tasks 目录。
- 把 `.uv-cache`、`.pytest_cache`、`node_modules` 等缓存当成项目结构。

## 快速判断

| 用户说法 | 场景 | 第一动作 |
| --- | --- | --- |
| “优化自由画布拖拽卡顿” | 场景一 | debugging，必要时 Graphify |
| “修复登录接口报错” | 场景一 | debugging + 最小验证 |
| “我也说不清，就是感觉不好用” | 场景一或二 | 深聊对齐，不写代码 |
| “构建自由画布模块” | 场景二 | Graphify + PRD 门禁 + OpenSpec |
| “新增 seedance 素材库模块” | 场景二 | 模块 PRD / OpenSpec / tasks |
| “新建一个画布应用” | 场景三 | 先问平台，再 bootstrap，再 PRD，再 Spec-Kit |
| “从零构建 AI 视频编辑项目” | 场景三 | 平台选择 + 项目级 bootstrap |

## 与复杂度预算的关系

- 场景一默认从轻量开始。
- 场景二默认模块级，通常从 medium 开始。
- 场景三默认 greenfield，即使 MVP 很小，也要有项目级 bootstrap 和 PRD 门禁。
