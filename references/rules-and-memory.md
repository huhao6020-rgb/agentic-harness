# 规则与记忆

## 设计原则

`AGENTS.md` / `CLAUDE.md` 不是几条零散提醒，而是项目的“智能体操作框架”。它要教会 Agent：

- 当前项目是什么，正在追求什么结果。
- 稳定规则、临时状态、长期记忆分别在哪里。
- 哪些上下文需要保留，哪些不要乱改。
- 什么时候必须深聊，什么时候可以轻量执行。
- 项目自定义规则和禁区是什么。

入口规则只放稳定工作方式、索引和硬门禁；任务历史、临时 todo、长篇 PRD 不塞进去。

## 分层

1. 平台入口：Codex 用 `AGENTS.md`，Claude Code 用 `CLAUDE.md`。
2. 状态层：`docs/agentic-dev/`。
3. 需求层：`docs/prd/` 和 `docs/agentic-dev/prd-gate.md`。
4. 记忆层：`memory/`。
5. 图谱层：`graphify-out/`。

## Codex: 结构化 `AGENTS.md` 模板

```md
# 项目智能体操作框架

## 1. 项目身份
- 项目名称：
- 项目类型：
- 当前阶段：
- 核心目标：
- 目标用户：
- 当前最重要的成功标准：

## 2. 平台与入口
- 当前平台：Codex
- 项目级 skills：`.agents/skills/`，不是项目内 `.codex/skills/`
- 状态入口：`docs/agentic-dev/INDEX.md`
- 当前状态：`docs/agentic-dev/ACTIVE.md`
- 初始化阶段报告：`docs/agentic-dev/bootstrap-report.md`
- 长期记忆：`memory/`

## 3. 启动读取顺序
每次开始任务先按顺序读取：

1. `docs/agentic-dev/INDEX.md`
2. `docs/agentic-dev/ACTIVE.md`
3. `docs/agentic-dev/bootstrap-report.md`
4. `docs/agentic-dev/prd-gate.md`
5. `docs/agentic-dev/spec-system.md`
6. `docs/agentic-dev/project-structure.md`
7. `docs/agentic-dev/graphify.md`
8. 中大型或长跑任务相关的 `docs/agentic-dev/TASKS.md`、`task-runner.md`、`handoff.md`、`verification-matrix.md`
9. 与当前任务相关的 `memory/` 或 `docs/prd/`

## 4. 语言与沟通
- 项目说明、计划、记忆、交接、验证记录默认使用简体中文。
- 文件名、命令、API、包名和工具名保留英文。
- 需求不清时先复述理解，再问关键问题，不直接实现。
- 中途用户改变方向时，先更新 `ACTIVE.md` / `prd-gate.md` / spec 状态，再继续。
- 用户要求“记录一下”“总结当前确认内容”“以后都按这个来”时，按“上下文同步与规则沉淀”判断写入位置。

## 5. 目的与深聊门禁
- tiny / small：确认一句话目标、范围、验收点。
- medium / large / greenfield：必须先用 `prd-test-writer` 或等价流程完成 PRD、用户故事、验收标准和测试基准。
- 未完成深聊对齐，不允许 Task Queue 长跑、多代理、Spec-Kit implement 或 OpenSpec apply。
- PRD 门禁状态以 `docs/agentic-dev/prd-gate.md` 为准。

## 6. 三场景开发路由
- 场景一：原有项目迭代、bug 修复，默认轻量；bug/性能/测试失败走 systematic-debugging。
- 场景二：当前项目单模块开发，先 Graphify 看边界，再 PRD 门禁，再 OpenSpec。
- 场景三：全新项目，先平台选择和 bootstrap，再 PRD 门禁，再 Spec-Kit。
- 小 bug、小文案、小配置不创建 PRD、Spec、Task Queue、多代理或 run ledger。

## 7. 规范系统
- 场景一/二 medium 以上按需 OpenSpec。
- 场景三默认 Spec-Kit。
- 同一任务只能使用 OpenSpec 或 Spec-Kit 之一。
- 当前规范系统状态以 `docs/agentic-dev/spec-system.md` 为准。

## 8. 项目结构规则
- 目录结构唯一来源是 `docs/agentic-dev/project-structure.md`。
- 新增 `apps/`、`src/`、`frontend/`、`backend/`、`packages/` 等目录前，必须先登记用途、边界和负责人。
- 不得创建多套未登记的平行结构。
- `.uv-cache`、`.pytest_cache`、`.uv-python`、`node_modules`、`.playwright-mcp` 是缓存或工具产物，不是业务结构。

## 9. 技术与代码规范
- 技术栈：
- 包管理器：
- 前端入口：
- 后端入口：
- API/数据层规则：
- 鉴权/权限规则：
- 命名规范：
- 禁止修改的文件或目录：
- 必须复用的本地工具、组件或封装：

## 10. Skill 路由
- 需求、PRD、测试基准：`prd-test-writer`。
- 架构、模块边界、调用链：Graphify。
- UI/UX、交互、视觉体验：`ui-ux-pro-max`。
- bug、异常、测试失败、性能问题：systematic-debugging。
- 实现计划：writing-plans。
- 多代理协作：subagent-driven-development，且必须有 tasks、write scope、verifier。
- 完成前：verification-before-completion。

## 11. 任务队列、多代理与长跑
- 长跑默认使用 `docs/agentic-dev/TASKS.md`，不是 `/goal`。
- `TASKS.md` 是唯一任务队列事实源；`task-runner.md` 是执行协议；`handoff.md` 是恢复入口。
- 只要存在 `ready` 或 `verify_failed` task，不得声称整体完成。
- 测试失败必须回流为 `verify_failed` 或拆分新的 `ready` task。
- 多代理必须从 `TASKS.md` 领取任务，且有互不重叠的 write scope 和独立 verifier。
- `/goal` 只在用户显式要求时使用，且仍必须以 `TASKS.md` 为事实源。
- 长任务每轮结束必须更新 `ACTIVE.md`、`TASKS.md`、`verification-matrix.md` 和 `handoff.md`。
- 不创建自定义 `.codex` goal、memory 或 tasks 目录。

## 12. 验证与完成标准
- 完成是达成用户目的，而不是只完成任务清单。
- 声称完成前必须提供测试、构建、lint、浏览器验收、OpenSpec verify 或等价证据。
- 无法验证时必须说明原因、风险和下一步。
- 完成后按需更新 `ACTIVE.md`、`prd-gate.md`、`spec-system.md`、`memory/`。

## 13. 上下文同步与规则沉淀
- `AGENTS.md` / `CLAUDE.md` 不是聊天纪要，只保存稳定操作规则、入口索引和硬门禁。
- 当前阶段、下一步、临时阻塞、当前 task 状态写入 `ACTIVE.md`、`TASKS.md` 或 `handoff.md`。
- 长期项目事实、模块边界、技术决策写入 `memory/`。
- 稳定工作方式、长期限制、项目禁区、目录规则、平台路径、完成标准写入 `AGENTS.md` / `CLAUDE.md`。
- 不确定是否稳定时，先写入 `ACTIVE.md` 的“待确认同步项”，不要直接污染入口规则。
- 每次用户要求记录共识后，必须说明：写入了哪里、为什么、哪些没有写入入口规则。

## 14. 记忆写入规则
- `memory/project.md`：长期项目事实、产品定位、技术栈、运行命令。
- `memory/decisions.md`：重要取舍、原因、日期、影响范围。
- `memory/constraints.md`：安全、性能、兼容性、业务禁区。
- `memory/glossary.md`：术语和业务对象。
- `memory/modules/<module>.md`：模块边界、入口、依赖、验证方式。
- 没有实质性新进展时，不要为了更新而改 memory。

## 15. 项目自定义规则
把只属于本项目的规则写在这里，例如：

- 业务禁区：
- UI 风格偏好：
- API 请求方式：
- 路由和权限规则：
- 状态管理规则：
- 数据库/缓存规则：
- 日志/监控规则：
- 部署规则：
- 不可接受结果：

## 16. 禁止事项
- 不要跳过 PRD 门禁直接长跑。
- 不要混用 OpenSpec 和 Spec-Kit。
- 不要绕过 `project-structure.md` 新建结构。
- 不要把临时任务历史堆进本文件。
- 不要在没有验证证据时声称完成。
```

## Claude Code: `CLAUDE.md` 模板

Claude-only 项目使用同样的结构，只把平台入口改为：

```md
## 2. 平台与入口
- 当前平台：Claude Code
- 项目级 skills：`.claude/skills/`
- 状态入口：`docs/agentic-dev/INDEX.md`
- 当前状态：`docs/agentic-dev/ACTIVE.md`
- 初始化阶段报告：`docs/agentic-dev/bootstrap-report.md`
- 长期记忆：`memory/`
```

双平台项目：

```md
@AGENTS.md

## Claude Code 适配
- 公共规则以 `AGENTS.md` 为准。
- Claude Code 项目级 skills 放 `.claude/skills/`。
- Claude Code 专属 hooks、agents、commands 放 `.claude/`。
- 不复制公共规则，避免 `AGENTS.md` 与 `CLAUDE.md` 漂移。
```

文件名必须是 `CLAUDE.md`，不要使用任何拼写错误的变体。

## 自定义规则采集问题

初始化前至少问一次：

```text
这个项目有没有必须遵守的特殊规则？
例如目录结构、接口方式、UI 偏好、权限、数据源、禁止修改范围、交付偏好或不可接受结果。
```

用户给出的项目约束优先写入 `AGENTS.md` / `CLAUDE.md` 的“项目自定义规则”；较长的事实写入 `memory/constraints.md` 或相关模块记忆。

## 上下文同步协议

当用户中途补充信息，尤其是说“帮我记录一下”“总结当前确认内容”“避免上下文太长忘记”“以后都按这个来”“这是项目规则”时，先判断信息归属，再写入对应位置：

| 信息类型 | 写入位置 | 不写入 |
| --- | --- | --- |
| 稳定工作方式、长期限制、项目禁区、目录规则、平台路径、完成标准 | `AGENTS.md` / `CLAUDE.md` | 临时聊天摘要 |
| 当前阶段、下一步、临时阻塞、当前 task 状态 | `ACTIVE.md` / `TASKS.md` / `handoff.md` | 入口规则 |
| 长期项目事实、模块边界、技术决策 | `memory/` | 临时 todo |
| 未确认偏好、猜测、一次性想法 | `ACTIVE.md` 的“待确认同步项” | `AGENTS.md` |

同步步骤：

1. 复述本次要记录的共识。
2. 判断每条信息是稳定规则、当前状态、长期事实还是待确认项。
3. 按表写入对应文件。
4. 如果改了 `AGENTS.md` / `CLAUDE.md`，避免复制整段聊天，只写可长期执行的规则。
5. 最终回复必须列出同步结果：写入了哪里、为什么、哪些没有写入入口规则。

示例：

- “以后新项目都用 Task Queue，不用 goal”：写入 `AGENTS.md` / `CLAUDE.md` 的长跑规则。
- “当前 T003 卡在 Playwright 安装失败”：写入 `ACTIVE.md` / `handoff.md`。
- “禁止新建平行 apps 目录”：写入入口规则，并同步 `project-structure.md` 或 `memory/constraints.md`。
- “这个 UI 风格更高级，先记一下”：未确认时写 `ACTIVE.md` 待确认；用户明确“以后都这样”才写入口规则。

## `memory/` 结构

```text
memory/
  project.md
  decisions.md
  constraints.md
  glossary.md
  modules/
```

不要把“今天的临时 todo”写入 memory；临时状态放 `ACTIVE.md`。
